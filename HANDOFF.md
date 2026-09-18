# HANDOFF.md — Operational Handoff Templates & Checklists

**Project:** OpsStrands — Secure Multi-Agent Developer Productivity Orchestrator  
**Event:** WeMakeDevs Bharat Builds Tour — “First Commit” Hackathon (17–20 Sept 2026)  
**Team (4 Members):** Aarth · Anurag · Naseer · Karthikeya  

---

> **Why This File Exists:**  
> In a 4-person, 3-day sprint with single-owner tracks, there is no pair-partner sitting beside you who already knows what you did. Every handoff below exists to move context out of your head and into this document so the other three owners (or an AI coding agent) can pick up seamlessly.

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
☐ If this touches the API contract (POST /task) or the authorize() signature:
    Did I ping the other affected owners before merging?
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
TESTS:          [How you verified — manual curl, unit test, browser flow]
RESULTS:        [Empirical results — e.g. "Cedar blocked Junior dev in 3.4ms"]
KNOWN_ISSUES:   [Any shortcut taken, pending edge case, or temporary hardcoding]
ARTIFACTS:      [Screenshots, sample JSON, or terminal output location]
NEXT_TASK:      [Immediate next logical step and who should execute it]
```

### Filled Example:
```markdown
TASK_ID:        SEC-002
STATUS:         DONE
WHAT_CHANGED:   Integrated `cedarpy` into `security/cedar_interceptor.py`. The interceptor
                now evaluates the 3 Cedar policies against the caller's Cognito role
                and target environment, returning an allow/deny tuple with reason.
FILES_CHANGED:  security/cedar_interceptor.py, security/test_cedar_interceptor.py
COMMANDS_RUN:   pytest security/test_cedar_interceptor.py -v
TESTS:          Executed all 5 test vectors from Build Guide §5.1. Verified that:
                1. Engineer can rollback staging.
                2. Engineer CANNOT rollback production (denied with policy #1 citation).
                3. SeniorEngineer CAN rollback production.
                4. SeniorEngineer CANNOT query billing (denied with hard forbid citation).
RESULTS:        5/5 test vectors passed. Average Cedar evaluation latency was 2.8ms.
KNOWN_ISSUES:   Currently reads Cedar policies from a local .cedar file; Amazon Verified
                Permissions cloud sync is deferred to post-demo.
ARTIFACTS:      Logs saved to `tests/output/cedar_eval_benchmarks.log`.
NEXT_TASK:      Wire `cedar_interceptor.py` into Anurag's Lambda handler (BE-003).
```

---

## 3. Daily Integration Handoff (Day 1 $\rightarrow$ Day 2 $\rightarrow$ Day 3)

Fill this out during the evening sync at the end of each hackathon day:

```markdown
DAY COMPLETED:          [ 1 | 2 | 3 ]
INTEGRATION CHECKPOINT: [ PASS | PARTIAL | FAIL ]
GIT TAG CREATED:        [ day1-checkpoint | day2-checkpoint | demo-ready-v1.0 ]

WHAT WORKS END-TO-END TODAY:
  - [ ] Example: Frontend logs in via Cognito and submits prompt to API Gateway.
  - [ ] Example: Strands agent parses prompt and invokes get_recent_errors.

WHAT IS STILL BROKEN OR MISSING:
  - [ ] Example: Visual timeline doesn't display the red deny badge yet.

DEPLOYED URLS (Verify reachable from mobile/incognito):
  - Frontend (Amplify): https://main.d123456abcdef.amplifyapp.com
  - Backend API: https://api.opsstrands.internal/prod/task

NEW ENV VARS INTRODUCED TODAY:
  - [List variable names only — NEVER paste secret values here]

TOMORROW'S TOP PRIORITIES:
  - Aarth (Frontend):
  - Anurag (Backend):
  - Naseer (Agentic AI):
  - Karthikeya (Security):

CRITICAL RISKS / BLOCKERS FOR TOMORROW:
  - [Anything that threatens the demo deadline]
```

---

## 4. Mid-Task / Blocker Handoff

If you get stuck, hit a wall, or must step away unexpectedly, leave this note:

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

If you are pairing with an agentic AI assistant (e.g., Antigravity, Claude Code), each session starts fresh. Before concluding an agent session, ensure the agent complies with the following:

```
☐ Updated PROJECT_STATUS.md for all tasks touched during the session.
☐ Did NOT redesign or deviate from the frozen architecture in OpsStrands_BUILD_GUIDE.md.
☐ Did NOT hallucinate or fake test results; ran actual commands in the terminal.
☐ Documented changed files, commands executed, and verified outputs.
☐ Did NOT push code to GitHub without running pre-commit checks.
☐ Left a crystal-clear NEXT_TASK for the human developer.
```

**Starting a New Session with an AI Agent:**  
Always provide:
1. The most recent entry in this file (`HANDOFF.md`).
2. `PROJECT_STATUS.md` (to see what is currently in progress).
3. The specific section of `OpsStrands_BUILD_GUIDE.md` corresponding to the current task.

---

## 6. Final Pre-Demo Presenter Handoff

**The Presenter's "Go-Bag" — Have this open on screen before walking up to demo:**

### 1. Demo User Credentials (Seeded in Cognito)
- **Senior Engineer (Authorized Demo Flow):**
  - Username: `senior.dev@opsstrands.internal`
  - Password: `Password@123`
  - Cognito Role: `SeniorEngineer`
- **Junior Engineer (Blocked Demo Flow):**
  - Username: `junior.dev@opsstrands.internal`
  - Password: `Password@123`
  - Cognito Role: `Engineer`

### 2. Live URLs
- **Amplify Web App:** `https://main.d123456abcdef.amplifyapp.com`
- **API Gateway Swagger/Docs:** `https://<api-id>.execute-api.us-east-1.amazonaws.com/prod/docs`

### 3. Emergency Revert & Offline Backup
- **Known-Good Git Commit:** `git checkout demo-ready-v1.0`
- **Backup Demo Video:** Saved locally at `artifacts/OpsStrands_3Min_Demo_Backup.mp4`
- **Offline Mode:** If venue Wi-Fi fails, switch frontend to `VITE_USE_LOCALSTACK=true` and demo locally against Docker LocalStack container.

### 4. 3-Minute Script Cues (From `OpsStrands_IDEA.md` §8)
- `0:00 - 0:30`: Highlight developer context switching & the Confused Deputy problem.
- `0:30 - 1:15`: Senior Engineer login $\rightarrow$ staging query & rollback $\rightarrow$ **Cedar ALLOW**.
- `1:15 - 2:00`: Junior Engineer login $\rightarrow$ production rollback $\rightarrow$ **Cedar DENY** (Highlight policy explanation).
- `2:00 - 2:35`: Hard Forbid prompt injection test $\rightarrow$ **Billing query blocked**.
- `2:35 - 3:00`: LocalStack terminal split-screen $\rightarrow$ Zero-Trust serverless summary.
