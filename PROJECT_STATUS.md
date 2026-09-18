# PROJECT_STATUS.md — Living Status Tracker

**Project:** OpsStrands — Secure Multi-Agent Developer Productivity Orchestrator  
**Event:** WeMakeDevs Bharat Builds Tour — “First Commit” Hackathon (17–20 Sept 2026)  
**Team (4 Members):** Aarth · Anurag · Naseer · Karthikeya  

> **This document is a snapshot of reality, not an aspirational plan.**  
> `OpsStrands_BUILD_GUIDE.md` specifies what *should* happen. This file records what *has actually been built, verified, or broken*, as of the timestamp below. Update this file at the start and end of every work session.

---

## Header — Update Every Session

```
LAST UPDATED:                 [2026-09-18 15:00 IST]
UPDATED BY:                   [Aarth / Anurag / Naseer / Karthikeya]
CURRENT PHASE:                [ Day 0 - Setup | Day 1 - Foundation | Day 2 - Integration | Day 3 - Polish & Demo ]
HOURS REMAINING UNTIL DEMO:   [ 48 ]
```

---

## 1. Quick Dashboard

*Status is readable in 5 seconds. Legend: 🔴 Not started · 🟡 In progress · 🟢 Verified & passing · ⚫ Blocked*

| Area | Status | Notes | Owner |
|---|---|---|---|
| **Frontend Deployed (Amplify)** | 🔴 Not started | React/Vite skeleton pending initial push | Aarth |
| **Cognito User Pool & Auth** | 🔴 Not started | Senior & Junior user pool definition ready | Aarth |
| **Backend (SAM + LocalStack)** | 🔴 Not started | `template.yaml` scaffolding in progress | Anurag |
| **DynamoDB Audit Table** | 🔴 Not started | Schema finalized in Build Guide §4.3 | Anurag |
| **Strands Agent (Bedrock)** | 🔴 Not started | Claude 3 Sonnet model ID confirmed | Naseer |
| **Operational Tools (MCP)** | 🔴 Not started | 4 core operational tools defined | Naseer |
| **AWS Cedar Engine (`cedarpy`)**| 🔴 Not started | 3 core policies written; tests passing locally | Karthikeya |
| **Full Closed-Loop Flow** | 🔴 Not started | Blocked until Day 2 integration | All 4 |
| **5 Consecutive Clean Demo Runs**| 🔴 Not started | Scheduled for Day 3 morning | All 4 |

---

## 2. Environment & Deployment Status

| Service / Component | Purpose | Target URL / Host | Status | Last Verified | Owner |
|---|---|---|---|---|---|
| **AWS Amplify Hosting** | Public Web UI | `https://main.<app-id>.amplifyapp.com` | 🔴 | — | Aarth |
| **Amazon Cognito** | User Authentication | `opsstrands-auth.auth.us-east-1...` | 🔴 | — | Aarth |
| **API Gateway (HTTP API)**| REST API Routing | `https://<api-id>.execute-api...` | 🔴 | — | Anurag |
| **LocalStack** | Local Serverless Dev | `http://localhost:4566` | 🔴 | — | Anurag |
| **DynamoDB Table** | Immutable Audit Trail | `opsstrands-audit-log` | 🔴 | — | Anurag |
| **Amazon Bedrock** | Agent Brain | `anthropic.claude-3-sonnet...` | 🔴 | — | Naseer |
| **AWS Cedar PDP** | Policy Decision Point| In-memory `cedarpy` engine in Lambda | 🔴 | — | Karthikeya |
| **GitHub Repository** | Source Control | `https://github.com/AarthGoyal123/OpsStrands` | 🟢 | 2026-09-18 | All 4 |

> **Security Reminder:** Never commit real API keys, passwords, or Cognito client secrets to this file or Git. Hostnames and IDs only.

---

## 3. Task Board

*Task IDs follow the format: `FE-xxx` (Frontend), `BE-xxx` (Backend), `AI-xxx` (Agent), `SEC-xxx` (Security), `CROSS-xxx` (Integration/Demo).*  
*Status values: `PENDING` $\rightarrow$ `IN_PROGRESS` $\rightarrow$ `BLOCKED` $\rightarrow$ `DONE`.*

### Frontend & Identity (Aarth)

| Task ID | Description | Status | Last Updated | Notes / PR Link |
|---|---|---|---|---|
| `FE-001` | Scaffold React/Vite app with TailwindCSS & Lucide icons | PENDING | | |
| `FE-002` | Configure Amazon Cognito Auth & Login Screen (Senior/Junior roles) | PENDING | | |
| `FE-003` | Deploy initial frontend to AWS Amplify Hosting (Live URL verification) | PENDING | | |
| `FE-004` | Build interactive natural language task input console | PENDING | | |
| `FE-005` | Build Visual Agent Timeline (Plan $\rightarrow$ Cedar Check $\rightarrow$ Execution) | PENDING | | |
| `FE-006` | Connect frontend to live API Gateway `POST /task` endpoint | PENDING | | |

---

### Backend Orchestration (Anurag)

| Task ID | Description | Status | Last Updated | Notes / PR Link |
|---|---|---|---|---|
| `BE-001` | Create SAM `template.yaml` (API Gateway, Lambda, DynamoDB) | PENDING | | |
| `BE-002` | Spin up LocalStack Docker container and verify `awslocal` connectivity | PENDING | | |
| `BE-003` | Implement Lambda handler entrypoint and request router | PENDING | | |
| `BE-004` | Provision DynamoDB table `opsstrands-audit-log` with TTL and indexes | PENDING | | |
| `BE-005` | Wire Strands agent module and Cedar interceptor into Lambda handler | PENDING | | |
| `BE-006` | Deploy backend stack to real AWS account via `sam deploy` | PENDING | | |

---

### Agentic AI & Bedrock (Naseer)

| Task ID | Description | Status | Last Updated | Notes / PR Link |
|---|---|---|---|---|
| `AI-001` | Verify Bedrock Claude 3 Sonnet access with `smoke_bedrock.py` | PENDING | | |
| `AI-002` | Implement Strands Agents SDK reasoning loop and system prompts | PENDING | | |
| `AI-003` | Implement MCP Tool 1: `get_recent_errors` (CloudWatch/LocalStack logs)| PENDING | | |
| `AI-004` | Implement MCP Tool 2: `get_deployment_status` (Revision health check)| PENDING | | |
| `AI-005` | Implement MCP Tool 3: `rollback_last_deployment` (Rollback engine) | PENDING | | |
| `AI-006` | Implement MCP Tool 4: `restart_service` & Bedrock model fallback logic | PENDING | | |

---

### Security & Platform (Karthikeya)

| Task ID | Description | Status | Last Updated | Notes / PR Link |
|---|---|---|---|---|
| `SEC-001` | Author 3 Cedar policies (`opsstrands.cedar`): RBAC, Hard-Deny, Read | PENDING | | |
| `SEC-002` | Build and verify `cedar_interceptor.py` using `cedarpy` | PENDING | | |
| `SEC-003` | Verify all 5 Cedar test vectors pass standalone evaluation | PENDING | | |
| `SEC-004` | Implement Cognito JWT claim extraction $\rightarrow$ Cedar entity mapping | PENDING | | |
| `SEC-005` | Benchmark and record Cedar evaluation latency ($p50 < 5\text{ms}$) | PENDING | | |
| `SEC-006` | Finalize AWS Builder Center blog post draft | PENDING | | |

---

### Cross-Cutting & Demo (All 4 Members)

| Task ID | Description | Status | Last Updated | Notes / PR Link |
|---|---|---|---|---|
| `CROSS-001` | Day 1 Integration Checkpoint: All 4 components run in isolation | PENDING | | |
| `CROSS-002` | Day 2 Integration Checkpoint: End-to-end loop verified on LocalStack & AWS | PENDING | | |
| `CROSS-003` | Day 3 Morning: 5 consecutive clean demo runs achieved | PENDING | | |
| `CROSS-004` | Record 3-minute demo video following exact script cues | PENDING | | |
| `CROSS-005` | Package final submission, verify public repo, submit before deadline | PENDING | | |

---

## 4. Component Status Mapping

| Component | Architecture Role | Owner | Current Status | Last Updated |
|---|---|---|---|---|
| **Cognito Identity Provider** | CORE MVP | Aarth | PENDING | |
| **React Task Console** | CORE MVP | Aarth | PENDING | |
| **Visual Policy Timeline** | CORE MVP | Aarth | PENDING | |
| **Amplify Hosting** | CORE MVP (Ship-It) | Aarth | PENDING | |
| **SAM CLI + LocalStack** | CORE MVP (Build-It)| Anurag | PENDING | |
| **API Gateway HTTP API** | CORE MVP | Anurag | PENDING | |
| **Orchestrator Lambda** | CORE MVP | Anurag | PENDING | |
| **DynamoDB Audit Trail** | CORE MVP | Anurag | PENDING | |
| **Strands Agent SDK Loop** | CORE MVP | Naseer | PENDING | |
| **Bedrock Claude 3 Sonnet** | CORE MVP | Naseer | PENDING | |
| **4 MCP Operational Tools** | CORE MVP | Naseer | PENDING | |
| **Bedrock Fallback Tier** | CORE MVP | Naseer | PENDING | |
| **Cedar Policy Decision Point**| CORE MVP (Innovation)| Karthikeya | PENDING | |
| **Hard Deny-List Guardrail** | CORE MVP (Innovation)| Karthikeya | PENDING | |
| **AWS Builder Center Blog** | ENHANCEMENT | Karthikeya | PENDING | |
| **EventBridge Async Fan-Out**| POST-DEMO | Anurag | POST-DEMO | Pruned for 3-day scope |
| **Firecracker MicroVMs** | POST-DEMO | Karthikeya | POST-DEMO | Pruned for 3-day scope |
| **OpenSearch Cluster** | POST-DEMO | Naseer | POST-DEMO | Pruned for 3-day scope |

---

## 5. Integration Checkpoints

### Day 1 Integration Checkpoint
```
[ ] Frontend: React scaffold boots; Cognito Hosted UI redirects cleanly
[ ] Frontend: Deployed on AWS Amplify Hosting (URL reachable from smartphone)
[ ] Backend: SAM local start-api serves /health against LocalStack
[ ] Backend: DynamoDB table opsstrands-audit-log created and queryable
[ ] Agent: Bedrock test script executes Claude 3 Sonnet call successfully
[ ] Agent: Strands Agent formats tool call for get_recent_errors locally
[ ] Security: cedarpy evaluates all 5 test vectors with 100% precision
[ ] GIT: Day 1 tagged as 'day1-checkpoint' and pushed to remote
```
**Checkpoint Result:** `[ PENDING / PASS / FAIL ]` — **Notes:**

---

### Day 2 Integration Checkpoint
```
[ ] Closed Loop: React UI -> API Gateway -> Lambda -> Strands -> Cedar -> DDB
[ ] Authorized Flow: SeniorEngineer requests staging rollback -> CEDAR ALLOWS -> Tool runs -> Audit logged
[ ] Blocked Flow: Junior Engineer requests prod rollback -> CEDAR BLOCKS -> Tool NOT run -> Reason displayed in UI
[ ] Hard Forbid Flow: Prompt injection attempting billing query -> CEDAR HARD FORBID -> Denied
[ ] Dual Track Parity: Complete flow tested on LocalStack (Build-It) AND AWS Cloud (Ship-It)
[ ] GIT: Day 2 tagged as 'day2-checkpoint' and pushed to remote
```
**Checkpoint Result:** `[ PENDING / PASS / FAIL ]` — **Notes:**

---

### Day 3 Pre-Demo Checkpoint
```
[ ] 5 consecutive clean runs completed without manual resets or unhandled errors
[ ] Cedar evaluation latency verified under 5ms (logged in CloudWatch/LocalStack)
[ ] Backup demo video (1080p, 3 minutes) recorded and stored in artifacts/
[ ] Public GitHub repository scrubbed of all secrets, credentials, and .env files
[ ] README.md finalized with honest architecture notes, benchmarks, and LocalStack steps
[ ] All 4 team members have reviewed and approved the submission
```
**Checkpoint Result:** `[ PENDING / PASS / FAIL ]` — **Notes:**

---

## 6. Blockers Log

| # | Blocker Description | Raised By | Date | Impact | Resolution |
|---|---|---|---|---|---|
| 1 | *(Example)* Bedrock Claude 3 Sonnet access pending AWS account approval | Naseer | 2026-09-18 | Blocks AI-001 | Enable model access in us-east-1 console or use Claude Haiku |

---

## 7. Cut Log (Deliberate Scope Pruning)

*Records features deliberately cut to protect the 3-day timeline. Show this to judges as proof of rigorous architectural triage.*

| # | Feature Cut | Original Tier | Reason for Cut | Approved Date |
|---|---|---|---|---|
| 1 | Firecracker MicroVM Execution | Research Candidate | Complex kernel setup; structured MCP tools provide complete isolation | 2026-09-18 |
| 2 | Corretto / Java Microservices | Optional Stack | Single Python 3.11 runtime reduces deployment friction by 80% | 2026-09-18 |
| 3 | OpenSearch Log Cluster | Infrastructure | DynamoDB indexed audit logs + CloudWatch queries satisfy all demo needs | 2026-09-18 |
| 4 | Multi-Agent Swarms (A2A) | Enhancement | Single orchestrator driving specialized MCP tools is rock-solid and demo-safe | 2026-09-18 |
| 5 | Async Step Functions / EventBridge| Architecture | Synchronous Lambda execution is fast (<3s) and simpler to demo live | 2026-09-18 |

---

## 8. Decisions Log

| Decision ID | Date | Architectural Decision | Rationale |
|---|---|---|---|
| `DEC-001` | 2026-09-18 | Use `cedarpy` in Lambda memory instead of Verified Permissions API | Eliminates a 40ms network hop, keeping Cedar policy evaluation under 5ms. |
| `DEC-002` | 2026-09-18 | Standardize all backend & agent code on Python 3.11 | Strands SDK and `cedarpy` both run natively in Python; eliminates cross-runtime IPC. |
| `DEC-003` | 2026-09-18 | Limit demo tools to exactly 4 operational MCP tools | 4 tools demonstrate read, health, staging mutation, and prod mutation without bloat. |
| `DEC-004` | 2026-09-18 | Vite + TailwindCSS on AWS Amplify Hosting | Zero-config continuous deployment with sub-minute build times for rapid UI updates. |

---

## 9. Reproducibility Run Log (5 Consecutive Clean Runs)

*Mandatory test protocol from `OpsStrands_BUILD_GUIDE.md` §7 before demo submission:*

| Run # | Timestamp (IST) | Environment | Senior Rollback (Allow) | Junior Rollback (Deny) | Hard Forbid Billing (Deny) | Cedar Latency | Outcome |
|---|---|---|---|---|---|---|---|
| 1 | | AWS Cloud | [ ] | [ ] | [ ] | ms | |
| 2 | | AWS Cloud | [ ] | [ ] | [ ] | ms | |
| 3 | | LocalStack | [ ] | [ ] | [ ] | ms | |
| 4 | | AWS Cloud | [ ] | [ ] | [ ] | ms | |
| 5 | | AWS Cloud | [ ] | [ ] | [ ] | ms | |
