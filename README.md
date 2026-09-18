# OpsStrands 🛡️🤖

> **Secure Multi-Agent Developer Productivity Orchestrator**  
> Built for the **WeMakeDevs Bharat Builds Tour — “First Commit” Hackathon (17–20 Sept 2026)**  
> **Repository:** [https://github.com/AarthGoyal123/OpsStrands](https://github.com/AarthGoyal123/OpsStrands)

---

## 🎯 The One-Line Pitch

> *“Other teams will show you an AI agent that can execute infrastructure commands. We show you an AI colleague that executes workflows autonomously — and then we try to make it break its own rules on stage, and watch AWS Cedar mathematically block it in microseconds.”*

OpsStrands bridges the gap between AI code generation and autonomous infrastructure remediation. Powered by the open-source **Strands Agents SDK** and **Amazon Bedrock**, OpsStrands diagnoses production anomalies, queries CloudWatch logs, and executes deployments through natural language.

Crucially, it solves the **Confused Deputy Problem** in agentic AI by inserting **AWS Cedar** (`cedarpy`) as an inline Policy Decision Point (PDP) in front of every tool execution, mathematically verifying the authenticated human's **Amazon Cognito** cryptographic identity and role before any infrastructure mutation can occur.

---

## 📑 Complete Documentation Suite

All planning, specifications, protocols, and workflows are codified in our core documentation suite:

1. 💡 **[OpsStrands_IDEA.md](OpsStrands_IDEA.md)** — The Strategic Design Record: 7-step analysis framework, root cause & confused deputy research (arXiv:2503.12188), baseline vs. winning innovation, Cedar policy architecture, 3-minute demo script, and AWS Builder Center blog draft.
2. 🛠️ **[OpsStrands_BUILD_GUIDE.md](OpsStrands_BUILD_GUIDE.md)** — The Operational Build Bible: 3-day phased roadmap, component architecture, exact `POST /task` and `authorize()` contracts, 4 MCP tools, Cedar policies, troubleshooting, and cut order.
3. ⚙️ **[ENVIRONMENT_SETUP.md](ENVIRONMENT_SETUP.md)** — Zero-to-Running Day 0 Guide: Local tooling prerequisites (Python 3.11, Node 18, Docker, SAM CLI), LocalStack configuration, Bedrock model access, `cedarpy` setup, and smoke tests.
4. 🌿 **[GIT_WORKFLOW.md](GIT_WORKFLOW.md)** — Team Collaboration Guide: 4-track branch model (`fe/*`, `be/*`, `ai/*`, `sec/*`), Task ID commit convention (`[BE-001]`, etc.), secrets safety protocol, and daily tagging.
5. 🤝 **[HANDOFF.md](HANDOFF.md)** — Operational Continuity: Solo Builder Self-Review Checklist, End-of-Session templates, AI coding agent pairing protocols, and pre-demo presenter go-bag.
6. 📊 **[PROJECT_STATUS.md](PROJECT_STATUS.md)** — Living Status Dashboard: 5-second health overview, 4-track task board, integration checkpoints, cut log, decision log, and 5-run reproducibility log.

---

## 🏛️ System Architecture

```
Developer (Browser)
      │ Login
      ▼
React App on AWS Amplify Hosting ────────► Amazon Cognito (Auth / JWT)
      │
      │ Natural Language Task + Bearer JWT
      ▼
Amazon API Gateway (HTTP API)
      │
      ▼
AWS Lambda Orchestrator
      │
      ├─► Strands Agent (Amazon Bedrock: Claude 3 Sonnet)
      │   Proposes Tool Execution (e.g. rollback_last_deployment)
      │
      ▼
AWS Cedar Policy Decision Point (cedarpy) ◄── Principal (from Cognito JWT)
      │                                       Action    (Action::"ExecuteAgentTool")
      │                                       Resource  (Tool::"DeployInfrastructure")
      │                                       Context   (environment: staging/prod)
      │
   ┌──┴──────────────────────────────┐
   │ ALLOW                           │ DENY
   ▼                                 ▼
Execute Tool via MCP Adapter      Block Execution, Explain Policy Violation
   │                                 │
   └───► DynamoDB Audit Table ◄──────┘
         (Session, Principal, Decision, Timestamp)
      │
      ▼
Visual Execution Timeline in React UI
```

---

## 🚀 Dual-Track Hackathon Alignment

- 💻 **Build It Track (Local & Open-Source):** Runs 100% offline on localhost using SAM CLI, LocalStack, Strands Agents SDK, and open-source Cedar (`cedarpy`).
- ☁️ **Ship It Track (Deployed Cloud URL):** Live deployment on AWS Amplify Hosting, API Gateway, AWS Lambda, Amazon DynamoDB, Amazon Cognito, and Amazon Bedrock.
