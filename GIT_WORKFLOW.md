# GIT_WORKFLOW.md — 4-Member Collaboration & Version Control

**Project:** OpsStrands — Secure Multi-Agent Developer Productivity Orchestrator  
**Event:** WeMakeDevs Bharat Builds Tour — “First Commit” Hackathon (17–20 Sept 2026)  
**Team (4 Members):** Aarth · Anurag · Naseer · Karthikeya  

---

> **Read Before Your First Commit:**  
> In a 3-day hackathon with 4 developers, bad git hygiene (merge conflicts, breaking someone else’s build, or accidentally pushing AWS credentials) will instantly derail your momentum. Follow this streamlined workflow to maintain speed without sacrificing stability.

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
├── frontend/             --> Owned by Aarth
├── backend/              --> Owned by Anurag
├── agent/                --> Owned by Naseer
├── security/             --> Owned by Karthikeya
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
# ==============================================================================
# SECRETS & ENVIRONMENT VARIABLES (CRITICAL)
# ==============================================================================
.env
.env.*
!.env.example
*.pem
*.key
*.pfx
*.cert
credentials
credentials.json
aws_credentials

# ==============================================================================
# PYTHON & AGENT DEPENDENCIES
# ==============================================================================
__pycache__/
*.py[cod]
*$py.class
.venv/
venv/
env/
*.egg-info/
dist/
build/
.pytest_cache/

# ==============================================================================
# NODE / REACT / VITE FRONTEND
# ==============================================================================
node_modules/
frontend/node_modules/
frontend/dist/
frontend/.amplify/
.amplify/
.vite/
npm-debug.log*
yarn-debug.log*

# ==============================================================================
# AWS SAM CLI & LOCALSTACK ARTIFACTS
# ==============================================================================
.aws-sam/
samconfig.toml
localstack/
*.sqlite3

# ==============================================================================
# OS & IDE NOISE
# ==============================================================================
.DS_Store
Thumbs.db
.vscode/
.idea/
*.swp
*.bak
/tmp/
*.log
```

**Verification:** Run `git status`. If `.env`, `node_modules/`, or `.venv/` appear under untracked files, DO NOT commit. Fix `.gitignore` first.

---

## 4. Branching Strategy: 4 Independent Tracks

```
main (Production Deploy / Protected)
  ▲
  ├─────── fe/aarth-task-ui          (Aarth: Frontend)
  ├─────── be/anurag-sam-lambda      (Anurag: Backend)
  ├─────── ai/naseer-strands-mcp     (Naseer: Agentic AI)
  └─────── sec/karthikeya-cedar-pdp  (Karthikeya: Security)
```

### Branch Naming Convention:
- `fe/<description>`: Frontend components, Amplify configs, UI styling.
- `be/<description>`: SAM CLI, Lambda handlers, API Gateway, DynamoDB.
- `ai/<description>`: Strands Agent prompts, Bedrock bindings, MCP tools.
- `sec/<description>`: Cedar policy specs, `cedarpy` interceptor, test vectors.
- `integration/<description>`: Multi-owner wiring sessions.

---

## 5. Commit Message Standards (Task-ID Tagging)

Every commit message must begin with the corresponding **Task ID** from `PROJECT_STATUS.md`:
`[<TRACK>-<NUMBER>] <Imperative verb> <Brief summary of change>`

### Examples:
```bash
# Frontend
git commit -m "[FE-002] Implement Visual Agent Timeline with allow/deny badges"

# Backend
git commit -m "[BE-001] Scaffold SAM template with LocalStack Lambda and API Gateway"

# Agentic AI
git commit -m "[AI-003] Add MCP operational tool get_recent_errors with log parser"

# Security & Platform
git commit -m "[SEC-001] Define Cedar RBAC policy for staging vs prod deployments"
```

**Why this matters:** When demoing or explaining changes to judges, you can immediately trace any git commit directly back to your technical specification and status tracker.

---

## 6. Cross-Cutting Changes & PR Protocol

Most work occurs in isolated folders, but changes to **shared surface areas** require cross-owner coordination before pushing to `main`:

### Shared Surface Areas:
1. `template.yaml` (SAM CLI deployment spec)
2. `POST /task` request/response JSON schema (Defined in `OpsStrands_BUILD_GUIDE.md` §4.1)
3. `authorize()` function signature (`OpsStrands_BUILD_GUIDE.md` §4.2)
4. `.env.example` additions

### Protocol:
1. Open a short-lived branch (e.g., `integration/update-api-contract`).
2. Post a quick diff in the team chat.
3. Once the affected owner (e.g., Aarth for UI or Anurag for Lambda) gives a thumbs-up, merge to `main`.
4. Run `git pull origin main` immediately on all 4 machines.

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
If an AWS Access Key or secret was pushed to a public or shared GitHub repo:
1. **IMMEDIATE STEP 1: Revoke the Key in AWS Console.**  
   Do not spend 10 minutes trying to rewrite git history first. Go straight to AWS IAM $\rightarrow$ Users $\rightarrow$ Security Credentials $\rightarrow$ Deactivate / Delete Access Key.
2. **STEP 2: Invalidate Bedrock / Third-party Tokens.**
3. **STEP 3: Scrub Git History.**
   Use `git-filter-repo` or BFG Repo-Cleaner:
   ```bash
   pip install git-filter-repo
   git filter-repo --path-glob '*.env' --invert-paths --force
   git push origin main --force
   ```
4. **STEP 4: Generate Fresh Keys** and update local `.env` files.

---

## 8. Daily Checkpoint Tagging

At the conclusion of each day's sync (after passing the Day's integration checkpoint), create an annotated Git tag on `main`:

```bash
# End of Day 1 (Foundation verified)
git checkout main
git pull origin main
git tag -a day1-checkpoint -m "Day 1 Complete: LocalStack, Amplify skeleton, Bedrock test, Cedar engine verified"
git push origin day1-checkpoint

# End of Day 2 (Full closed loop integrated)
git tag -a day2-checkpoint -m "Day 2 Complete: Full end-to-end loop running with Cedar interception and timeline UI"
git push origin day2-checkpoint

# Day 3 (Demo freeze)
git tag -a demo-ready-v1.0 -m "Day 3 Complete: 5 consecutive clean demo runs, video recorded, submission locked"
git push origin demo-ready-v1.0
```

These tags provide instant, known-good fallback states if an experiment goes wrong during demo preparations!
