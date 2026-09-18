# PROJECT_STATUS.md — Living Status Tracker

**Project:** OpsStrands — Autonomous Civic Hazard & Emergency Triage Orchestrator  
**Event:** WeMakeDevs Bharat Builds Tour — “First Commit” Hackathon (17–20 Sept 2026)  
**Repository:** [https://github.com/AarthGoyal123/OpsStrands](https://github.com/AarthGoyal123/OpsStrands)  
**Team (4 Members):** Aarth · Anurag · Naseer · Karthikeya  

> **This document is a snapshot of reality, not an aspirational plan.**  
> `OpsStrands_BUILD_GUIDE.md` specifies what *should* happen. This file records what *has actually been built, verified, or broken*, as of the timestamp below. Update this file at the start and end of every work session.

---

## Header — Update Every Session

```
LAST UPDATED:                 [2026-09-19 00:05 IST]
UPDATED BY:                   [Aarth / Anurag / Naseer / Karthikeya]
CURRENT PHASE:                [ Day 0 - Setup | Day 1 - Foundation | Day 2 - Integration | Day 3 - Polish & Demo ]
HOURS REMAINING UNTIL DEMO:   [ 42 ]
```

---

## 1. Quick Dashboard

*Status is readable in 5 seconds. Legend: 🔴 Not started · 🟡 In progress · 🟢 Verified & passing · ⚫ Blocked*

| Area | Status | Notes | Owner |
|---|---|---|---|
| **Mobile PWA Deployed (Amplify)** | 🔴 Not started | React/Vite PWA pending initial deployment | Aarth |
| **Interactive Hazard Map** | 🔴 Not started | Leaflet component with red/amber danger circles | Aarth |
| **Cognito User Pool (3 Roles)** | 🔴 Not started | Citizen, WardVolunteer, MunicipalOfficer roles | Aarth |
| **Backend (SAM + LocalStack)** | 🔴 Not started | SAM template with DynamoDB Geo-Store scaffolded | Anurag |
| **DynamoDB Geo-Store** | 🔴 Not started | Schema finalized with geohash indexing and TTL | Anurag |
| **Strands Agent (Bedrock Multimodal)**| 🔴 Not started | Claude 3 Sonnet multimodal prompt design ready | Naseer & Karthikeya |
| **MCP Spatial Clustering Tool** | 🔴 Not started | 500m geofence radius aggregation tool | Naseer & Karthikeya |
| **Cedar Policy Engine (`cedarpy`)** | 🔴 Not started | 3 civic governance policies passing tests | Karthikeya |
| **Full Corroboration Closed-Loop** | 🔴 Not started | Blocked until Day 2 integration | All 4 |
| **5 Consecutive Clean Demo Runs** | 🔴 Not started | Scheduled for Day 3 morning | All 4 |

---

## 2. Environment & Deployment Status

| Service / Component | Purpose | Target URL / Host | Status | Last Verified | Owner |
|---|---|---|---|---|---|
| **AWS Amplify Hosting** | Mobile PWA & Map | `https://main.<app-id>.amplifyapp.com` | 🔴 | — | Aarth |
| **Amazon Cognito** | 3-Role Auth Pool | `opsstrands-civic.auth.us-east-1...` | 🔴 | — | Aarth |
| **API Gateway (HTTP API)**| Incident Submission | `https://<api-id>.execute-api...` | 🔴 | — | Anurag |
| **LocalStack** | Offline Disaster Drills | `http://localhost:4566` | 🔴 | — | Anurag |
| **DynamoDB Geo-Store**| Geotagged Hazard Store | `opsstrands-civic-incidents` | 🔴 | — | Anurag |
| **Amazon Bedrock** | Multimodal Vision/Voice| `anthropic.claude-3-sonnet...` | 🔴 | — | Naseer |
| **AWS Cedar PDP** | Anti-Panic Policy Gate | In-memory `cedarpy` engine in Lambda | 🔴 | — | Karthikeya |
| **GitHub Repository** | Source Control | `https://github.com/AarthGoyal123/OpsStrands`| 🟢 | 2026-09-18 | All 4 |

---

## 3. Task Board

*Task IDs: `FE-xxx` (Frontend), `BE-xxx` (Backend), `AI-xxx` (Agentic AI), `SEC-xxx` (Security/Policy), `CROSS-xxx` (Cross-cutting).*  
*Status values: `PENDING` $\rightarrow$ `IN_PROGRESS` $\rightarrow$ `BLOCKED` $\rightarrow$ `DONE`.*

### Frontend & Mobile PWA (Aarth)

| Task ID | Description | Status | Last Updated | Notes / PR Link |
|---|---|---|---|---|
| `FE-001` | Scaffold React/Vite PWA with mobile camera & audio recorder | PENDING | | |
| `FE-002` | Implement Interactive Ward Hazard Map (Leaflet / danger circles) | PENDING | | |
| `FE-003` | Deploy initial PWA to AWS Amplify Hosting (Verify mobile URL) | PENDING | | |
| `FE-004` | Configure Cognito Auth with Citizen/Volunteer toggle | PENDING | | |
| `FE-005` | Build Visual Alert Banner (Amber Hold vs. Red Active Alert) | PENDING | | |
| `FE-006` | Connect PWA to live API Gateway `POST /report` & `GET /hazards` | PENDING | | |

---

### Backend & Geospatial Architecture (Anurag)

| Task ID | Description | Status | Last Updated | Notes / PR Link |
|---|---|---|---|---|
| `BE-001` | Scaffold SAM `template.yaml` (API Gateway, Lambda, DynamoDB) | PENDING | | |
| `BE-002` | Configure LocalStack container and verify `awslocal` table creation | PENDING | | |
| `BE-003` | Provision DynamoDB Geo-Store `opsstrands-civic-incidents` with TTL | PENDING | | |
| `BE-004` | Implement Lambda router handling `POST /report` and `GET /hazards` | PENDING | | |
| `BE-005` | Wire Strands agent and Cedar interceptor into Lambda handler | PENDING | | |
| `BE-006` | Build offline disaster drill simulation runner script | PENDING | | |

---

### Agentic AI & Multimodal Triage (Naseer & Karthikeya)

| Task ID | Description | Status | Last Updated | Notes / PR Link |
|---|---|---|---|---|
| `AI-001` | Verify Bedrock Claude 3 multimodal call with sample flood image | PENDING | | |
| `AI-002` | Author Strands system prompt for Hindi voice & image triage | PENDING | | |
| `AI-003` | Implement MCP Tool 1: `cluster_nearby_reports` (500m geofence) | PENDING | | |
| `AI-004` | Implement MCP Tool 2: `assess_hazard_severity` (Depth & Sentiment) | PENDING | | |
| `AI-005` | Implement MCP Tool 3: `log_civic_incident` (Save to Geo-Store) | PENDING | | |
| `AI-006` | Implement MCP Tool 4: `propose_ward_alert` (Formulate alert payload)| PENDING | | |

---

### Security, Cedar Governance & Platform (Karthikeya)

| Task ID | Description | Status | Last Updated | Notes / PR Link |
|---|---|---|---|---|
| `SEC-001` | Author 3 Cedar policies: Corroboration consensus, Anti-panic forbid, Read | PENDING | | |
| `SEC-002` | Build `cedar_civic_interceptor.py` using `cedarpy` bindings | PENDING | | |
| `SEC-003` | Verify all 5 Cedar civic test vectors pass standalone evaluation | PENDING | | |
| `SEC-004` | Co-lead Strands MCP tool execution integration with Naseer | PENDING | | |
| `SEC-005` | Benchmark and record Cedar evaluation latency ($p50 < 4\text{ms}$) | PENDING | | |
| `SEC-006` | Finalize 3-minute pitch script and AWS Builder Center blog post draft | PENDING | | |

---

### Cross-Cutting & Demo (All 4 Members)

| Task ID | Description | Status | Last Updated | Notes / PR Link |
|---|---|---|---|---|
| `CROSS-001` | Day 1 Integration Checkpoint: Components run in local isolation | PENDING | | |
| `CROSS-002` | Day 2 Integration Checkpoint: 3-report corroboration triggers alert | PENDING | | |
| `CROSS-003` | Day 3 Morning: 5 consecutive clean demo runs completed | PENDING | | |
| `CROSS-004` | Record 3-minute demo video following exact cue sheet | PENDING | | |
| `CROSS-005` | Package final submission on WeMakeDevs hackathon portal | PENDING | | |

---

## 4. Component Status Mapping

| Component | Architecture Role | Owner | Current Status | Last Updated |
|---|---|---|---|---|
| **Mobile PWA Reporting** | CORE MVP | Aarth | PENDING | |
| **Interactive Hazard Map** | CORE MVP | Aarth | PENDING | |
| **Cognito Multi-Role Auth**| CORE MVP | Aarth | PENDING | |
| **Amplify Hosting** | CORE MVP (Ship-It) | Aarth | PENDING | |
| **SAM CLI + LocalStack** | CORE MVP (Build-It)| Anurag | PENDING | |
| **DynamoDB Geo-Store** | CORE MVP | Anurag | PENDING | |
| **Strands Agent SDK Loop** | CORE MVP | Naseer & Karthikeya | PENDING | |
| **Bedrock Multimodal (Vision/Voice)**| CORE MVP | Naseer | PENDING | |
| **4 MCP Operational Tools** | CORE MVP | Naseer & Karthikeya | PENDING | |
| **Cedar Consensus Gate** | CORE MVP (Innovation)| Karthikeya | PENDING | |
| **Anti-Panic Forbid Rule** | CORE MVP (Innovation)| Karthikeya | PENDING | |
| **AWS Builder Center Blog** | ENHANCEMENT | Karthikeya | PENDING | |
| **Automated Municipal SMS Fan-Out**| POST-DEMO | Anurag | POST-DEMO | Pruned for 3-day scope |
| **Firecracker MicroVMs** | POST-DEMO | Karthikeya | POST-DEMO | Pruned for 3-day scope |
| **OpenSearch Geo-Cluster** | POST-DEMO | Naseer | POST-DEMO | Pruned for 3-day scope |

---

## 5. Integration Checkpoints

### Day 1 Integration Checkpoint
```
[ ] Frontend: React PWA scaffold boots; camera & microphone permissions work
[ ] Frontend: Deployed on AWS Amplify Hosting (URL loads on smartphone)
[ ] Backend: SAM local start-api serves /hazards against LocalStack
[ ] Backend: DynamoDB table opsstrands-civic-incidents created
[ ] Agent: Bedrock multimodal script executes Claude 3 vision call
[ ] Security: cedarpy evaluates all 5 test vectors with 100% accuracy
[ ] GIT: Day 1 tagged as 'day1-checkpoint' and pushed to remote
```

---

### Day 2 Integration Checkpoint
```
[ ] Closed Loop: PWA -> API Gateway -> Lambda -> Strands -> Cedar -> DDB -> Map
[ ] Anti-Panic Hold: Single report submitted -> Cedar BLOCKS public alert -> Held
[ ] Consensus Trigger: Reports #2 and #3 arrive -> Cedar ALLOWS alert -> Map turns RED
[ ] Volunteer Flow: Ward Volunteer submits report -> Immediate Cedar ALLOW
[ ] Dual Track: Flow works on LocalStack (Build-It) AND AWS Cloud (Ship-It)
[ ] GIT: Day 2 tagged as 'day2-checkpoint' and pushed to remote
```

---

### Day 3 Pre-Demo Checkpoint
```
[ ] 5 consecutive clean runs completed without unhandled errors
[ ] Cedar evaluation latency verified under 4ms
[ ] Backup demo video (1080p, 3 minutes) recorded
[ ] Public GitHub repository scrubbed of secrets and .env files
[ ] README.md finalized with civic narrative and LocalStack steps
[ ] All 4 team members approve final submission
```

---

## 6. Cut Log (Deliberate Scope Pruning)

| # | Feature Cut | Original Tier | Reason for Cut | Approved Date |
|---|---|---|---|---|
| 1 | Firecracker MicroVM Execution | Research Candidate | Complex kernel setup; MCP tools provide clean isolation | 2026-09-18 |
| 2 | OpenSearch Log Cluster | Infrastructure | DynamoDB geohash indexing handles 500m radius queries in <25ms | 2026-09-18 |
| 3 | Automated SMS / Siren Hardware | Enhancement | Simulated on-screen alert banner satisfies demo proof | 2026-09-18 |
| 4 | Live Drone Feed Ingestion | Research Candidate | Citizen mobile photo/voice is 100x more relatable for the theme | 2026-09-18 |

---

## 7. Decisions Log

| Decision ID | Date | Architectural Decision | Rationale |
|---|---|---|---|
| `DEC-001` | 2026-09-18 | Pivot from IT server DevOps to Civic Hazard Orchestration | Perfect alignment with Bharat Builds theme ("family, community, country"). |
| `DEC-002` | 2026-09-18 | Use Cedar as a consensus gate rather than a role checker | Eliminates AgentCore competition; introduces novel decentralized civic governance. |
| `DEC-003` | 2026-09-18 | Karthikeya co-leads Strands Agent MCP integration with Naseer | Strengthens AI throughput and couples policy interception directly to agent tool loops. |
| `DEC-004` | 2026-09-18 | Decouple public read path from LLM reasoning | `/hazards` queries DynamoDB directly in <15ms; Bedrock runs asynchronously for triage. |

---

## 8. Reproducibility Run Log (5 Consecutive Clean Runs)

| Run # | Timestamp (IST) | Environment | Single Report (Hold) | 3 Reports (Allow) | Volunteer (Allow) | Cedar Latency | Outcome |
|---|---|---|---|---|---|---|---|
| 1 | | AWS Cloud | [ ] | [ ] | [ ] | ms | |
| 2 | | AWS Cloud | [ ] | [ ] | [ ] | ms | |
| 3 | | LocalStack | [ ] | [ ] | [ ] | ms | |
| 4 | | AWS Cloud | [ ] | [ ] | [ ] | ms | |
| 5 | | AWS Cloud | [ ] | [ ] | [ ] | ms | |
