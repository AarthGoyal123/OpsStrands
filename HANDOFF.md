# HANDOFF.md — Operational Handoff Templates & Checklists

**Project:** OpsStrands — Autonomous Civic Hazard & Emergency Triage Orchestrator  
**Event:** WeMakeDevs Bharat Builds Tour — “First Commit” Hackathon (17–20 Sept 2026)  
**Repository:** [https://github.com/AarthGoyal123/OpsStrands](https://github.com/AarthGoyal123/OpsStrands)  
**Team (4 Members):** Aarth · Anurag · Naseer · Karthikeya  

---

> **Why This File Exists:**  
> In a 4-person, 3-day sprint, there is no pair-partner sitting beside you who already knows what you did. Every handoff below exists to move context out of your head and into this document so the other three owners (or an AI coding agent) can pick up seamlessly.

---

## Table of Contents

1. [Solo Builder Self-Review Checklist](#1-solo-builder-self-review-checklist)
2. [End-of-Session Handoff Template](#2-end-of-session-handoff-template)
3. [Daily Integration Handoff (Day 1 $\rightarrow$ Day 2 $\rightarrow$ Day 3)](#3-daily-integration-handoff-day-1-rightarrow-day-2-rightarrow-day-3)
4. [Mid-Task / Blocker Handoff](#4-mid-task--blocker-handoff)
5. [AI Coding Agent Session Handoff](#5-ai-coding-agent-session-handoff)
6. [Final Pre-Demo Presenter Handoff](#6-final-pre-demo-presenter-handoff)

---

## 1. Solo Builder Self-Review Checklist

Run this checklist on your local environment before pushing any code to `main`:

```
☐ Does it run locally from a clean git pull without undocumented manual steps?
☐ Did I execute the verification smoke test for my track (OpsStrands_BUILD_GUIDE.md §8)?
☐ Did I inspect server logs / browser console for hidden errors or uncaught exceptions?
☐ If this touches the API contract (POST /report or GET /hazards) or authorize() signature:
    Did I coordinate with the affected owner before merging?
☐ Did I verify no AWS keys, Bedrock tokens, or .env files are staged (git diff --cached)?
☐ Did I update PROJECT_STATUS.md with the Task ID and updated status?
☐ If stopping work for the day, did I fill in an End-of-Session Handoff entry (§2)?
```

---

## 2. End-of-Session Handoff Template

Fill this out every time you complete a task or step away from your workstation:

```markdown
TASK_ID:        [e.g., SEC-002]
STATUS:         [ DONE | IN_PROGRESS | BLOCKED ]
WHAT_CHANGED:   [1–3 concise sentences explaining what the system now does]
FILES_CHANGED:  [List of paths modified or created]
COMMANDS_RUN:   [Exact commands used to execute and verify]
TESTS:          [How you verified — curl, unit test, PWA mobile flow]
RESULTS:        [Empirical results — e.g. "Cedar blocked single prank report in 2.9ms"]
KNOWN_ISSUES:   [Any shortcut taken, pending edge case, or temporary hardcoding]
ARTIFACTS:      [Screenshots, sample JSON, or terminal output location]
NEXT_TASK:      [Immediate next logical step and who should execute it]
```

### Filled Example:
```markdown
TASK_ID:        SEC-002
STATUS:         DONE
WHAT_CHANGED:   Integrated `cedarpy` into `security/cedar_civic_interceptor.py`. Evaluates
                the 3 civic governance policies against the report corroboration count,
                preventing single-citizen panic alerts while permitting multi-citizen alerts.
FILES_CHANGED:  security/cedar_civic_interceptor.py, tests/test_cedar_civic.py
COMMANDS_RUN:   pytest tests/test_cedar_civic.py -v
TESTS:          Tested all 5 civic test vectors:
                1. Single citizen report -> Blocked by Policy #1 (Hold).
                2. 3 citizen reports within 500m -> Permitted by Policy #1 (Alert Active).
                3. Ward Volunteer single report -> Permitted by Policy #1 (Verified).
                4. Citizen attempting rescue dispatch -> Hard Forbid by Policy #2.
                5. Anonymous hazard map query -> Permitted by Policy #3.
RESULTS:        5/5 test vectors passed. Cedar evaluation latency was 2.8ms.
KNOWN_ISSUES:   Currently reads policies from local file; remote sync is post-demo.
ARTIFACTS:      Output saved to `tests/output/cedar_civic_benchmarks.log`.
NEXT_TASK:      Wire interceptor into Anurag's Lambda handler (BE-003).
```

---

## 3. Daily Integration Handoff (Day 1 $\rightarrow$ Day 2 $\rightarrow$ Day 3)

```markdown
DAY COMPLETED:          [ 1 | 2 | 3 ]
INTEGRATION CHECKPOINT: [ PASS | PARTIAL | FAIL ]
GIT TAG CREATED:        [ day1-checkpoint | day2-checkpoint | demo-ready-v1.0 ]

WHAT WORKS END-TO-END TODAY:
  - [ ] Example: Citizen PWA uploads voice memo + photo to API Gateway.
  - [ ] Example: Strands agent classifies Hindi voice note into FlashFlooding.

WHAT IS STILL BROKEN OR MISSING:
  - [ ] Example: Map doesn't auto-refresh when report #3 is submitted.

DEPLOYED URLS (Verify reachable from mobile/incognito):
  - Frontend (Amplify): https://main.d123456abcdef.amplifyapp.com
  - Backend API: https://api.opsstrands.internal/prod/report

NEW ENV VARS INTRODUCED TODAY:
  - [List variable names only — NEVER paste secret values here]

TOMORROW'S TOP PRIORITIES:
  - Aarth (Frontend):
  - Anurag (Backend):
  - Naseer (Agentic AI):
  - Karthikeya (Security & Policy):

CRITICAL RISKS / BLOCKERS FOR TOMORROW:
  - [Anything that threatens the demo deadline]
```

---

## 4. Mid-Task / Blocker Handoff

```markdown
TASK_ID:                  [e.g., AI-003]
EXACT POINT OF FAILURE:   [File, function, line number, or exact error trace]
WHAT I ALREADY TRIED:     [Document failed attempts so others don't repeat them]
WHAT I SUSPECT IS WRONG:  [Your current hypothesis]
CAN THIS WAIT?            [ YES (wait for me) | NO (someone please take over) ]
IF HANDING OFF TO SOMEONE ELSE:
  [Specific context, edge cases, or API nuances they need to know]
```

---

## 5. AI Coding Agent Session Handoff

When pairing with an AI assistant (Antigravity, Claude Code), ensure the agent complies with:

```
☐ Updated PROJECT_STATUS.md for all tasks touched during the session.
☐ Did NOT redesign or deviate from the frozen architecture in OpsStrands_BUILD_GUIDE.md.
☐ Did NOT hallucinate or fake test results; ran actual commands in the terminal.
☐ Documented changed files, commands executed, and verified outputs.
☐ Did NOT push code to GitHub without running pre-commit checks.
☐ Left a crystal-clear NEXT_TASK for the human developer.
```

---

## 6. Final Pre-Demo Presenter Handoff

**The Presenter's "Go-Bag" for Stage & Video Recording:**

### 1. Demo User Credentials (Seeded in Cognito)
- **Ordinary Citizen (Single-Report Demo Beat - Blocked):**
  - Username: `citizen.delhi@opsstrands.internal`
  - Password: `Password@123`
  - Role: `Citizen`
- **Ward Volunteer (Immediate Verification Demo Beat):**
  - Username: `volunteer.ward04@opsstrands.internal`
  - Password: `Password@123`
  - Role: `WardVolunteer`
- **Municipal Officer (Disaster Authority Beat):**
  - Username: `officer.disaster@opsstrands.internal`
  - Password: `Password@123`
  - Role: `MunicipalOfficer`

### 2. Live URLs
- **Amplify Mobile PWA:** `https://main.<app-id>.amplifyapp.com`
- **API Gateway Health:** `https://<api-id>.execute-api.us-east-1.amazonaws.com/prod/hazards`

### 3. Emergency Revert & Offline Disaster Simulation
- **Known-Good Git Commit:** `git checkout demo-ready-v1.0`
- **Backup Demo Video:** Saved locally at `artifacts/OpsStrands_Civic_3Min_Demo.mp4`
- **Offline Mode:** If venue Wi-Fi fails, run LocalStack on localhost and demo the offline disaster drill runner script: `python scripts/demo_disaster_drill.py`.

### 4. 3-Minute Script Cues (From `OpsStrands_IDEA.md` §9)
- `0:00 - 0:30`: Real photo of Indian underpass flooding $\rightarrow$ the dilemma of unverified rumors vs. panic alerts.
- `0:30 - 1:15`: Mobile PWA Hindi voice note upload $\rightarrow$ Bedrock multimodal triage.
- `1:15 - 2:00`: Single report submitted $\rightarrow$ **Cedar BLOCKS public panic alert** (Explain policy hold).
- `2:00 - 2:30`: Reports #2 and #3 arrive $\rightarrow$ **Cedar ALLOWS alert** $\rightarrow$ Interactive map illuminates with detour.
- `2:30 - 3:00`: LocalStack offline drill terminal split-screen $\rightarrow$ Serverless zero-idle cost summary.
