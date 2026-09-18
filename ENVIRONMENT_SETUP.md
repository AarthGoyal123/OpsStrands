# ENVIRONMENT_SETUP.md — Zero-to-Running (Day 0 Setup)

**Project:** OpsStrands — Autonomous Civic Hazard & Emergency Triage Orchestrator  
**Event:** WeMakeDevs Bharat Builds Tour — “First Commit” Hackathon (17–20 Sept 2026)  
**Repository:** [https://github.com/AarthGoyal123/OpsStrands](https://github.com/AarthGoyal123/OpsStrands)  
**Team (4 Members):**  
- **Aarth:** Frontend & Mobile PWA  
- **Anurag:** Backend & Geospatial Architecture  
- **Naseer:** Agentic AI (Multimodal Triage)  
- **Karthikeya:** Security, Cedar Governance & Agentic Co-Lead  

---

> **Day 0 Objective:**  
> Complete every setup step below **before Day 1 morning**. Ensure LocalStack, Python 3.11, Node 18, and `cedarpy` are confirmed operational with the smoke tests in §8.

---

## Table of Contents

1. [Local Tooling Prerequisites](#1-local-tooling-prerequisites)
2. [Cloud Accounts & Access](#2-cloud-accounts--access)
3. [LocalStack & Docker Setup (Build-It Track)](#3-localstack--docker-setup-build-it-track)
4. [Python & Agentic Environment Setup](#4-python--agentic-environment-setup)
5. [AWS Cedar (`cedarpy`) Setup](#5-aws-cedar-cedarpy-setup)
6. [Frontend & Map Environment (Ship-It Track)](#6-frontend--map-environment-ship-it-track)
7. [Environment Variables (`.env.example`)](#7-environment-variables-envexample)
8. [Track-by-Track Verification Smoke Tests](#8-track-by-track-verification-smoke-tests)

---

## 1. Local Tooling Prerequisites

| Tool | Version | Purpose | Verification Command |
|---|---|---|---|
| **Python** | `3.11.x` | Backend Lambda, Strands SDK, Cedar engine | `python --version` (or `python3`) |
| **Node.js** | `18.x` or `20.x` LTS | Frontend React PWA, Leaflet Map | `node -v` |
| **npm** / **pnpm** | `9.x+` | Package manager | `npm -v` |
| **Docker Desktop** | `24.x+` | Running LocalStack container | `docker --version` |
| **AWS CLI v2** | `2.15+` | AWS service management | `aws --version` |
| **AWS SAM CLI** | `1.110+` | Local serverless testing & cloud deploy | `sam --version` |
| **Git** | `2.40+` | Source control | `git --version` |

---

## 2. Cloud Accounts & Access

| Service | Purpose | Who Needs Setup | Free-Tier / Setup Notes |
|---|---|---|---|
| **AWS Account** | Hosting Bedrock, Lambda, DynamoDB, Cognito | Team Account | Enable Bedrock Claude 3 Sonnet & Haiku in `us-east-1` |
| **Amazon Bedrock** | Multimodal photo & Hindi voice triage | Naseer & Karthikeya | Go to AWS Console $\rightarrow$ Bedrock $\rightarrow$ Model Access $\rightarrow$ Request Claude 3 |
| **Amazon Cognito** | User Pool (Citizen, Volunteer, Municipal Officer) | Aarth & Anurag | Created via SAM template with custom role attribute |
| **AWS Amplify** | Public mobile web app hosting | Aarth | Linked to GitHub `main` branch |
| **GitHub** | Shared repository | All 4 Members | `https://github.com/AarthGoyal123/OpsStrands` |

---

## 3. LocalStack & Docker Setup (Build-It Track)

Run LocalStack to emulate DynamoDB, Lambda, and API Gateway on localhost:

```bash
docker run -d \
  --name localstack-opsstrands \
  -p 4566:4566 \
  -p 4510-4559:4510-4559 \
  -e SERVICES=lambda,apigateway,dynamodb,logs \
  localstack/localstack:latest
```

Verify LocalStack is responsive:
```bash
pip install awscli-local
awslocal dynamodb list-tables
# Expected output: { "TableNames": [] }
```

---

## 4. Python & Agentic Environment Setup

```bash
# Navigate to repository root
cd OpsStrands

# Create and activate virtual environment
python -m venv .venv

# On Windows:
.venv\Scripts\Activate.ps1
# On macOS/Linux:
source .venv/bin/activate

# Install dependencies
pip install --upgrade pip
pip install strands-agents cedarpy boto3 fastapi uvicorn pydantic pytest
```

---

## 5. AWS Cedar (`cedarpy`) Setup

Verify that `cedarpy` executes local Cedar policies:

```bash
python -c "
import cedarpy
policy = 'permit(principal, action, resource);'
res = cedarpy.is_authorized({'id': 'u1', 'type': 'Citizen'}, {'id': 'Report', 'type': 'Action'}, {'id': 'Ward', 'type': 'Resource'}, {}, policy, {})
print('Cedar Engine:', res)
"
```
**Expected Output:** `Cedar Engine: Decision.Allow`

---

## 6. Frontend & Map Environment (Ship-It Track)

The frontend is a lightweight React PWA with interactive maps and camera capture:

```bash
cd frontend
npm install
npm install leaflet react-leaflet lucide-react @aws-amplify/ui-react aws-amplify
npm run dev
# Starts local development server at http://localhost:5173
```

---

## 7. Environment Variables (`.env.example`)

Copy this template to `.env` locally (never commit `.env`):

```bash
# AWS Region & Credentials (for Bedrock live access)
AWS_REGION=us-east-1
AWS_ACCESS_KEY_ID=AKIA_YOUR_LOCAL_TEST_KEY
AWS_SECRET_ACCESS_KEY=YOUR_LOCAL_SECRET_KEY

# Amazon Bedrock Model IDs
BEDROCK_MODEL_ID=anthropic.claude-3-sonnet-20240229-v1:0
BEDROCK_HAIKU_MODEL_ID=anthropic.claude-3-haiku-20240307-v1:0

# Amazon Cognito Settings
COGNITO_USER_POOL_ID=us-east-1_example123
COGNITO_APP_CLIENT_ID=exampleclientid456

# LocalStack & Database
LOCALSTACK_ENDPOINT=http://localhost:4566
USE_LOCALSTACK=true
DYNAMODB_TABLE_NAME=opsstrands-civic-incidents

# Frontend Base URL
VITE_API_BASE_URL=http://localhost:3000/prod
```

---

## 8. Track-by-Track Verification Smoke Tests

### 8.1 Aarth (Frontend & PWA Smoke Test)
```bash
cd frontend
npm run build
npm run preview
# Open http://localhost:4173 — verify mobile view, camera button, and Leaflet map render
```

### 8.2 Anurag (Backend & LocalStack Smoke Test)
```bash
# Start SAM local API against LocalStack
sam local start-api --docker-network host

# In another terminal:
curl http://localhost:3000/hazards?ward_id=WARD-DEL-04
# Expected: {"ward_id": "WARD-DEL-04", "active_hazards": []}
```

### 8.3 Naseer & Karthikeya (Agent Multimodal Smoke Test)
Run `scripts/smoke_multimodal.py`:
```python
import boto3, json

client = boto3.client("bedrock-runtime", region_name="us-east-1")
prompt = "Citizen voice transcript: 'Underpass me paani bhar gaya hai, gaadi phas gayi'. Classify hazard_type and severity."
body = json.dumps({
    "anthropic_version": "bedrock-2023-05-31",
    "max_tokens": 150,
    "messages": [{"role": "user", "content": prompt}]
})

response = client.invoke_model(
    modelId="anthropic.claude-3-haiku-20240307-v1:0",
    body=body
)
print("Bedrock Triage:", json.loads(response["body"].read())["content"][0]["text"])
```
```bash
python scripts/smoke_multimodal.py
```

### 8.4 Karthikeya (Cedar Civic Governance Smoke Test)
Run `scripts/smoke_cedar_civic.py`:
```python
import cedarpy

policies = """
permit(
    principal,
    action == Action::"BroadcastCivicAlert",
    resource == Ward::"LocalZone"
) when { context.corroborated_reports_count >= 3 };
"""

# Test 1: Single report -> DENIED (Anti-panic hold)
res1 = cedarpy.is_authorized(
    {"id": "cit_1", "type": "Citizen"},
    {"id": "BroadcastCivicAlert", "type": "Action"},
    {"id": "LocalZone", "type": "Ward"},
    {"corroborated_reports_count": 1},
    policies,
    {}
)
assert str(res1) == "Decision.Deny", "Test 1 Failed"

# Test 2: 3 reports -> ALLOWED (Consensus reached)
res2 = cedarpy.is_authorized(
    {"id": "cit_1", "type": "Citizen"},
    {"id": "BroadcastCivicAlert", "type": "Action"},
    {"id": "LocalZone", "type": "Ward"},
    {"corroborated_reports_count": 3},
    policies,
    {}
)
assert str(res2) == "Decision.Allow", "Test 2 Failed"

print("✅ Cedar Civic Governance Smoke Test PASSED! Consensus gates operational.")
```
```bash
python scripts/smoke_cedar_civic.py
# Expected: ✅ Cedar Civic Governance Smoke Test PASSED! Consensus gates operational.
```
