# GIT_WORKFLOW.md — 4-Member Collaboration & Version Control

**Project:** OpsStrands — Autonomous Civic Hazard & Emergency Triage Orchestrator  
**Event:** WeMakeDevs Bharat Builds Tour — “First Commit” Hackathon (17–20 Sept 2026)  
**Repository:** [https://github.com/AarthGoyal123/OpsStrands](https://github.com/AarthGoyal123/OpsStrands)  
**Team (4 Members):** Aarth · Anurag · Naseer · Karthikeya  

---

> **Read Before Your First Commit:**  
> In a 3-day hackathon with 4 developers, maintaining disciplined branch hygiene and preventing secrets leaks is critical. Follow this streamlined workflow.

---

## Table of Contents

1. [The 4-Member Repository Model](#1-the-4-member-repository-model)
2. [Local Git Identity Setup](#2-local-git-identity-setup)
3. [The Ironclad `.gitignore`](#3-the-ironclad-gitignore)
4. [Branching Strategy: 4 Independent Tracks](#4-branching-strategy-4-independent-tracks)
5. [Commit Message Standards (Task-ID Tagging)](#5-commit-message-standards-task-id-tagging)
6. [Cross-Cutting Changes & PR Protocol](#6-cross-cutting-changes--pr-protocol)
7. [Secrets Safety & Accidental Leak Emergency Procedure](#7-secrets-safety--accidental-leak-emergency-procedure)
8. [Daily Checkpoint Tagging](#8-daily-checkpoint-tagging)

---

## 1. The 4-Member Repository Model

We maintain **one central GitHub repository** representing the hackathon build. All 4 members are added as collaborators with push access.

```
Main GitHub Repository:
https://github.com/AarthGoyal123/OpsStrands.git
├── frontend/             --> Owned by Aarth (React PWA, Map, Camera)
├── backend/              --> Owned by Anurag (SAM CLI, Lambda, DynamoDB)
├── agent/                --> Owned by Naseer & Karthikeya (Strands SDK, Bedrock)
├── security/             --> Owned by Karthikeya (Cedar Policies, cedarpy)
└── template.yaml         --> SAM Template (Managed by Anurag)
```

Because each owner works almost exclusively within their own directory, collision risks are minimized.

---

## 2. Local Git Identity Setup

Before committing, ensure your local git configuration matches your GitHub account credentials:

```bash
git config --global user.name "Your Full Name"
git config --global user.email "your-github-verified-email@example.com"

# Verify settings
git config --list | grep user
```

---

## 3. The Ironclad `.gitignore`

Place this `.gitignore` at the repository root immediately. **Never commit without it.**

```gitignore
# SECRETS & ENVIRONMENT VARIABLES
.env
.env.*
!.env.example
*.pem
*.key
credentials
aws_credentials

# PYTHON & AGENT DEPENDENCIES
__pycache__/
*.py[cod]
.venv/
venv/
*.egg-info/
dist/
build/
.pytest_cache/

# NODE / REACT FRONTEND
node_modules/
frontend/node_modules/
frontend/dist/
frontend/.amplify/
.vite/

# AWS SAM & LOCALSTACK
.aws-sam/
samconfig.toml
localstack/

# OS & IDE NOISE
.DS_Store
Thumbs.db
.vscode/
.idea/
*.log
```

---

## 4. Branching Strategy: 4 Independent Tracks

```
main (Production Deploy / Protected)
  ▲
  ├─────── fe/aarth-hazard-pwa       (Aarth: Frontend & Map)
  ├─────── be/anurag-geo-lambda      (Anurag: SAM & DynamoDB Geo-Store)
  ├─────── ai/naseer-strands-triage  (Naseer & Karthikeya: Agentic AI Core)
  └─────── sec/karthikeya-cedar-pdp  (Karthikeya: Cedar Civic Policies)
```

### Branch Naming Convention:
- `fe/<description>`: Mobile PWA, camera upload, Leaflet map markers.
- `be/<description>`: SAM CLI, Lambda handlers, API Gateway, DynamoDB.
- `ai/<description>`: Strands Agent prompts, Bedrock multimodal, MCP tools.
- `sec/<description>`: Cedar policy specs, `cedarpy` interceptor, test vectors.
- `integration/<description>`: Multi-owner wiring sessions.

---

## 5. Commit Message Standards (Task-ID Tagging)

Every commit message must begin with the corresponding **Task ID** from `PROJECT_STATUS.md`:
`[<TRACK>-<NUMBER>] <Imperative verb> <Brief summary of change>`

### Examples:
```bash
# Frontend
git commit -m "[FE-002] Implement Leaflet hazard map with active red danger pins"

# Backend
git commit -m "[BE-001] Scaffold SAM template with LocalStack DynamoDB Geo-Store"

# Agentic AI
git commit -m "[AI-003] Add MCP operational tool cluster_nearby_reports with geohashing"

# Security & Platform
git commit -m "[SEC-001] Define Cedar corroboration policy requiring 3 citizen reports"
```

---

## 6. Cross-Cutting Changes & PR Protocol

Most work occurs in isolated folders, but changes to **shared surface areas** require cross-owner coordination before pushing to `main`:

### Shared Surface Areas:
1. `template.yaml` (SAM CLI deployment spec)
2. `POST /report` and `GET /hazards` JSON schemas (`OpsStrands_BUILD_GUIDE.md` §4)
3. `authorize()` function signature (`OpsStrands_BUILD_GUIDE.md` §4.2)
4. `.env.example` additions

---

## 7. Secrets Safety & Accidental Leak Emergency Procedure

### 7.1 Pre-Commit Sanity Check
Before running `git commit`, always execute:
```bash
git diff --cached --stat
```
Inspect the list of staged files. If you see `.env` or any file containing `AKIA...` or private keys, unstage immediately:
```bash
git reset HEAD <leaked-file>
```

### 7.2 Emergency Protocol: If an AWS Key Was Pushed to GitHub
1. **IMMEDIATE STEP 1: Revoke the Key in AWS Console.** (IAM $\rightarrow$ Deactivate Access Key).
2. **STEP 2: Invalidate Bedrock / Third-party Tokens.**
3. **STEP 3: Scrub Git History** using `git-filter-repo`.
4. **STEP 4: Generate Fresh Keys** and update local `.env` files.

---

## 8. Daily Checkpoint Tagging

At the conclusion of each day's sync (after passing the Day's integration checkpoint), create an annotated Git tag on `main`:

```bash
# End of Day 1 (Foundation verified)
git checkout main
git pull origin main
git tag -a day1-checkpoint -m "Day 1 Complete: LocalStack Geo-Store, Amplify skeleton, Bedrock multimodal, Cedar engine verified"
git push origin day1-checkpoint

# End of Day 2 (Full closed loop integrated)
git tag -a day2-checkpoint -m "Day 2 Complete: Full end-to-end loop running with Cedar corroboration gate and live map"
git push origin day2-checkpoint

# Day 3 (Demo freeze)
git tag -a demo-ready-v1.0 -m "Day 3 Complete: 5 consecutive clean demo runs, video recorded, submission locked"
git push origin demo-ready-v1.0
```
