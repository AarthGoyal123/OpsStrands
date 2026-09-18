# ENVIRONMENT_SETUP.md — Zero-to-Running (Day 0 Setup)

**Project:** OpsStrands — Secure Multi-Agent Developer Productivity Orchestrator  
**Event:** WeMakeDevs Bharat Builds Tour — “First Commit” Hackathon (17–20 Sept 2026)  
**Team (4 Members):** Aarth (Frontend) · Anurag (Backend) · Naseer (AI) · Karthikeya (Security)  

---

> **Day 0 Rule:**  
> Complete everything in this file **before Day 1 morning**. Troubleshooting a missing Python wheel or Docker port collision on Day 1 eats into core build time you cannot recover. Do a 15-minute team sync where each person confirms their smoke tests pass.

---

## Table of Contents

1. [Local Tooling Prerequisites](#1-local-tooling-prerequisites)
2. [Cloud Accounts & Access](#2-cloud-accounts--access)
3. [LocalStack & Docker Setup (Build-It Track)](#3-localstack--docker-setup-build-it-track)
4. [Python & Agentic Environment Setup](#4-python--agentic-environment-setup)
5. [AWS Cedar (`cedarpy`) Setup](#5-aws-cedar-cedarpy-setup)
6. [Frontend Environment (Ship-It Track)](#6-frontend-environment-ship-it-track)
7. [Environment Variables (`.env.example`)](#7-environment-variables-envexample)
8. [Track-by-Track Verification Smoke Tests](#8-track-by-track-verification-smoke-tests)

---

## 1. Local Tooling Prerequisites

Every team member must install and verify the following core tools:

| Tool | Version | Purpose | Verification Command |
|---|---|---|---|
| **Python** | `3.11.x` | Backend Lambda, Strands SDK, Cedar engine | `python --version` (or `python3`) |
| **Node.js** | `18.x` or `20.x` LTS | Frontend React / Vite, Amplify CLI | `node -v` |
| **npm** / **pnpm** | `9.x+` | Package manager | `npm -v` |
| **Docker Desktop** | `24.x+` | Running LocalStack container | `docker --version` |
| **AWS CLI v2** | `2.15+` | Managing AWS services & LocalStack | `aws --version` |
| **AWS SAM CLI** | `1.110+` | Local serverless testing & cloud deploy | `sam --version` |
| **Git** | `2.40+` | Source control (see `GIT_WORKFLOW.md`) | `git --version` |

### Installation Quick-Links:
- **Windows (winget):**
  ```powershell
  winget install Python.Python.3.11
  winget install OpenJS.NodeJS.LTS
  winget install Docker.DockerDesktop
  winget install Amazon.AWSCLI
  winget install Amazon.SAMCLI
  winget install Git.Git
  ```
- **macOS (Homebrew):**
  ```bash
  brew install python@3.11 node awscli aws-sam-cli git
  brew install --cask docker
  ```

---

## 2. Cloud Accounts & Access

| Service | Purpose | Who Needs Setup | Free-Tier / Credit Notes |
|---|---|---|---|
| **AWS Account** | Hosting Bedrock, Lambda, DynamoDB, Cognito | Team Account (Anurag & Karthikeya manage IAM) | Use hackathon AWS credits; enable Bedrock model access in `us-east-1` |
| **Amazon Bedrock** | Foundation model inference (Claude 3 Sonnet) | Naseer (AI Owner) | Go to AWS Console $\rightarrow$ Bedrock $\rightarrow$ Model Access $\rightarrow$ Request Claude 3 Sonnet & Haiku |
| **Amazon Cognito** | User Pool & JWT token issuance | Aarth & Anurag | Created via SAM / AWS Console; free tier covers 50,000 MAUs |
| **AWS Amplify** | Hosting the public React web app | Aarth (Frontend Owner) | Continuous deployment connected to GitHub `main` branch |
| **GitHub** | Shared source code repository | All 4 Members | `https://github.com/AarthGoyal123/OpsStrands` (All 4 added as collaborators) |

---

## 3. LocalStack & Docker Setup (Build-It Track)

LocalStack emulates AWS serverless primitives on your laptop, enabling 100% offline development at zero cost.

### 3.1 Start LocalStack Container
Run the official LocalStack Docker container with ports exposed:

```bash
docker run -d \
  --name localstack-opsstrands \
  -p 4566:4566 \
  -p 4510-4559:4510-4559 \
  -e SERVICES=lambda,apigateway,dynamodb,logs \
  -e DOCKER_HOST=unix:///var/run/docker.sock \
  localstack/localstack:latest
```

*(On Windows PowerShell, use `docker run -d --name localstack-opsstrands -p 4566:4566 -e SERVICES=lambda,apigateway,dynamodb,logs localstack/localstack:latest`)*

### 3.2 Install `awscli-local` (Convenient Alias)
```bash
pip install awscli-local
```
Test that LocalStack is responsive:
```bash
awslocal dynamodb list-tables
# Expected output: { "TableNames": [] }
```

---

## 4. Python & Agentic Environment Setup

Set up a unified Python 3.11 virtual environment for backend and AI components.

### 4.1 Initialize Virtual Environment
```bash
# Navigate to repository root
cd OpsStrands

# Create virtual environment
python -m venv .venv

# Activate environment:
# Windows (PowerShell):
.venv\Scripts\Activate.ps1
# macOS/Linux:
source .venv/bin/activate
```

### 4.2 Install Required Dependencies
Create or use `backend/requirements.txt`:
```txt
# Core AWS SDK
boto3>=1.34.0
botocore>=1.34.0

# Agentic AI
strands-agents>=0.1.0
anthropic>=0.18.0

# Security & Policy Engine
cedarpy>=0.3.0

# Web & Local Server
fastapi>=0.110.0
uvicorn>=0.28.0
pydantic>=2.6.0
python-jose[cryptography]>=3.3.0
requests>=2.31.0

# Testing
pytest>=8.0.0
```

Install via pip:
```bash
pip install --upgrade pip
pip install -r backend/requirements.txt
```

---

## 5. AWS Cedar (`cedarpy`) Setup

OpsStrands uses `cedarpy`, the high-performance Python bindings to the official Rust implementation of AWS Cedar.

### 5.1 Verification of `cedarpy`
Run a quick inline Python test to confirm Cedar's native bindings are working:

```bash
python -c "
import cedarpy
policies = 'permit(principal, action, resource);'
decision = cedarpy.is_authorized(
    {'id': 'user1', 'type': 'User'},
    {'id': 'view', 'type': 'Action'},
    {'id': 'doc1', 'type': 'Document'},
    {},
    policies,
    {}
)
print('Cedar Engine Status:', decision)
"
```
**Expected Output:**
`Cedar Engine Status: Decision.Allow`

*(If `cedarpy` wheel installation fails on your OS, ensure you have Microsoft C++ Build Tools on Windows or `build-essential` on Linux/macOS).*

---

## 6. Frontend Environment (Ship-It Track)

The frontend is a fast, responsive Single Page Application built with React, Vite, and TailwindCSS.

### 6.1 Initialize Frontend Directory
```bash
cd frontend
npm install
```

### 6.2 Key Frontend Dependencies (`frontend/package.json`)
```json
{
  "dependencies": {
    "@aws-amplify/ui-react": "^6.1.0",
    "aws-amplify": "^6.0.0",
    "lucide-react": "^0.350.0",
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  },
  "devDependencies": {
    "@vitejs/plugin-react": "^4.2.0",
    "autoprefixer": "^10.4.18",
    "postcss": "^8.4.35",
    "tailwindcss": "^3.4.1",
    "vite": "^5.1.0"
  }
}
```

### 6.3 Local Frontend Server
```bash
npm run dev
# Starts local Vite server at http://localhost:5173
```

---

## 7. Environment Variables (`.env.example`)

Never commit `.env` files to git. Copy this template to `.env` locally:

```bash
# ==============================================================================
# OpsStrands Environment Configuration
# ==============================================================================

# AWS Credentials (For local testing against real Bedrock)
AWS_REGION=us-east-1
AWS_ACCESS_KEY_ID=AKIA_YOUR_LOCAL_TEST_KEY
AWS_SECRET_ACCESS_KEY=YOUR_LOCAL_SECRET_KEY

# Amazon Bedrock Settings
BEDROCK_MODEL_ID=anthropic.claude-3-sonnet-20240229-v1:0
BEDROCK_FALLBACK_MODEL_ID=anthropic.claude-3-haiku-20240307-v1:0

# Amazon Cognito Settings
COGNITO_USER_POOL_ID=us-east-1_example123
COGNITO_APP_CLIENT_ID=exampleclientid456
COGNITO_DOMAIN=https://opsstrands-auth.auth.us-east-1.amazoncognito.com

# Backend & LocalStack Endpoints
LOCALSTACK_ENDPOINT=http://localhost:4566
USE_LOCALSTACK=true
DYNAMODB_AUDIT_TABLE=opsstrands-audit-log

# Frontend API URL
VITE_API_BASE_URL=http://localhost:3000/prod
# When deployed to AWS, this becomes:
# VITE_API_BASE_URL=https://<api-id>.execute-api.us-east-1.amazonaws.com/prod
```

---

## 8. Track-by-Track Verification Smoke Tests

Before claiming Day 0 is done, every owner must execute their designated smoke test:

### 8.1 Aarth (Frontend Smoke Test)
```bash
cd frontend
npm run build
npm run preview
# Open browser at http://localhost:4173 — verify header and login modal render
```

### 8.2 Anurag (Backend / LocalStack Smoke Test)
```bash
# 1. Start SAM local API connected to LocalStack
sam local start-api --docker-network host

# 2. In another terminal, hit the health check
curl http://localhost:3000/health
# Expected: {"status": "HEALTHY", "mode": "LocalStack"}
```

### 8.3 Naseer (Agentic AI / Bedrock Smoke Test)
Create and run `scripts/smoke_bedrock.py`:
```python
import boto3, json

client = boto3.client("bedrock-runtime", region_name="us-east-1")
body = json.dumps({
    "anthropic_version": "bedrock-2023-05-31",
    "max_tokens": 100,
    "messages": [{"role": "user", "content": "Reply with exactly: Bedrock is online"}]
})

response = client.invoke_model(
    modelId="anthropic.claude-3-haiku-20240307-v1:0",
    body=body
)
result = json.loads(response["body"].read())
print("Bedrock Output:", result["content"][0]["text"])
```
```bash
python scripts/smoke_bedrock.py
# Expected: Bedrock Output: Bedrock is online
```

### 8.4 Karthikeya (Security / Cedar Smoke Test)
Create and run `scripts/smoke_cedar.py`:
```python
import cedarpy

policies = """
permit(
    principal,
    action == Action::"ExecuteAgentTool",
    resource == Tool::"DeployInfrastructure"
) when { context.environment == "staging" };
"""

# Test 1: Staging allow
res1 = cedarpy.is_authorized(
    {"id": "usr_dev", "type": "User"},
    {"id": "ExecuteAgentTool", "type": "Action"},
    {"id": "DeployInfrastructure", "type": "Tool"},
    {"environment": "staging"},
    policies,
    {}
)
assert str(res1) == "Decision.Allow", "Test 1 Failed"

# Test 2: Production deny
res2 = cedarpy.is_authorized(
    {"id": "usr_dev", "type": "User"},
    {"id": "ExecuteAgentTool", "type": "Action"},
    {"id": "DeployInfrastructure", "type": "Tool"},
    {"environment": "production"},
    policies,
    {}
)
assert str(res2) == "Decision.Deny", "Test 2 Failed"

print("✅ Cedar Policy Engine Smoke Test PASSED! Microsecond checks operational.")
```
```bash
python scripts/smoke_cedar.py
# Expected: ✅ Cedar Policy Engine Smoke Test PASSED! Microsecond checks operational.
```

When all 4 smoke tests output green, proceed to **Day 1 execution** in `OpsStrands_BUILD_GUIDE.md`!
