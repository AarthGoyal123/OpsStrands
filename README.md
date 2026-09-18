# OpsStrands 🛡️🌊

> **Autonomous Civic Hazard & Emergency Triage Orchestrator**  
> Built for the **WeMakeDevs Bharat Builds Tour — “First Commit” Hackathon (17–20 Sept 2026)**  
> **Repository:** [https://github.com/AarthGoyal123/OpsStrands](https://github.com/AarthGoyal123/OpsStrands)

---

## 🎯 The One-Line Pitch

> *“When monsoon rains flood an Indian road underpass or a live high-tension wire snaps on a street, citizens turn to unverified WhatsApp groups while official municipal helplines jam. We built OpsStrands: an autonomous civic hazard orchestrator that turns citizen voice notes and photos into verified emergency alerts. Crucially, giving AI the power to trigger public alerts creates a deadly risk of prank-induced panic. OpsStrands solves this by using AWS Cedar to mathematically enforce statutory municipal governance—requiring multi-citizen geofenced consensus before any public alert can be broadcast.”*

---

## 🚨 The Problem: Life-Threatening Hazards & Information Chaos

Every monsoon season across Indian cities and towns, innocent citizens lose their lives to submerged road underpasses, open manholes hidden under rainwater, and snapped electrical cables. 

Existing solutions fail when emergencies strike:
- **Municipal 311 Apps & Portals:** Clunky 15-field bureaucratic forms requiring ward numbers citizens don't know. Completely inaccessible to delivery riders, auto drivers, or frantic commuters driving in torrential rain.
- **Social Media (WhatsApp & Twitter/X):** High reach, but complete chaos. Outdated photos from previous years are forwarded as current news, causing panic and spreading misinformation without geo-coordinates.
- **The AI Dilemma:** If you give an autonomous AI agent the power to broadcast public alerts or dispatch emergency response teams, a single prank report (e.g. uploading a fake image of a collapsed bridge) could trigger citywide panic and waste vital disaster machinery.

---

## 💡 The Solution: Multimodal Triage + AWS Cedar Civic Governance

OpsStrands bridges the gap between chaotic crowdsourced reporting and structured municipal action:

1. **Zero-Barrier Citizen Reporting:** An ordinary commuter simply speaks a 5-second voice note in Hindi or English (*"Underpass me 3 foot paani bhar gaya hai, gaadiyan phas rahi hain"*) or snaps a photo.
2. **Strands Multimodal Agent:** Powered by the open-source **Strands Agents SDK** and **Amazon Bedrock (Claude 3)**, the agent extracts the hazard type (`FlashFlooding`, `LiveWire`, `OpenManhole`), classifies physical danger, and clusters nearby reports.
3. **AWS Cedar Policy Gate (The Core Innovation):** Before any public alert can be broadcast:
   - Single unverified report $\rightarrow$ **Blocked & Held** (prevents prank-induced panic).
   - $\ge 3$ distinct citizen reports within a 500m geofence OR certified Ward Volunteer verification $\rightarrow$ **Permitted**.
4. **Live Community Hazard Map:** Verified hazards immediately illuminate the community map with red danger perimeters and safe detour recommendations.

---

## 📑 Complete Documentation Suite

All specifications, architectural blueprints, and workflows are codified in our core documentation:

1. 💡 **[OpsStrands_IDEA.md](OpsStrands_IDEA.md)** — Strategic Design Record: Real-world civic crisis analysis, deep pros/cons research, baseline vs. winning innovation, Cedar consensus policy architecture, 3-minute winning demo script, and AWS Builder Center blog draft.
2. 🛠️ **[OpsStrands_BUILD_GUIDE.md](OpsStrands_BUILD_GUIDE.md)** — Technical Build Bible: 3-day phased roadmap, system architecture, exact `POST /report` and `GET /hazards` API schemas, 4 MCP operational tools, Cedar policies, and cut order.
3. ⚙️ **[ENVIRONMENT_SETUP.md](ENVIRONMENT_SETUP.md)** — Day 0 Zero-to-Running Guide: Local tooling prerequisites (Python 3.11, Node 18, Docker, SAM CLI), LocalStack configuration, Bedrock multimodal setup, `cedarpy` verification, and smoke tests.
4. 🌿 **[GIT_WORKFLOW.md](GIT_WORKFLOW.md)** — Team Collaboration Guide: 4-track branch model (`fe/*`, `be/*`, `ai/*`, `sec/*`), Task ID commit conventions, secrets safety protocol, and daily checkpoint tags.
5. 🤝 **[HANDOFF.md](HANDOFF.md)** — Operational Continuity: Solo Builder Self-Review Checklist, End-of-Session templates, AI coding agent protocols, and pre-demo presenter go-bag.
6. 📊 **[PROJECT_STATUS.md](PROJECT_STATUS.md)** — Living Status Dashboard: 5-second health overview, 4-track task board, integration checkpoints, cut log, decision log, and 5-run reproducibility log.

---

## 🏛️ System Architecture

```
Citizen on Street (Browser / PWA)
      │
      ├─► Snaps Photo + Hindi Voice Memo + GPS
      ▼
React PWA on AWS Amplify Hosting
      │
      ▼ POST /report
Amazon API Gateway (HTTP API)
      │
      ▼
AWS Lambda Orchestrator
      │
      ├─► Amazon Bedrock (Claude 3 Multimodal)
      │   Transcribes Hindi Audio & Analyzes Submersion Depth
      │
      ├─► Strands Agents SDK (MCP Operational Tools)
      │   Tool: cluster_nearby_reports(lat, lng, 500m)
      │   Queries DynamoDB Geo-Store for Local Reports
      │
      ▼
AWS Cedar Policy Decision Point (cedarpy)
      │ Evaluates Statutory Civic Governance:
      │ Is corroborated_reports_count >= 3 OR is WardVolunteer?
      │
   ┌──┴──────────────────────────────┐
   │ ALLOW (Consensus Met)           │ DENY (Single Report Hold)
   ▼                                 ▼
Broadcast Public Ward Alert       Hold Alert Pending Verification
Save as VERIFIED_ALERT            Save as PENDING_CORROBORATION
   │                                 │
   └───► DynamoDB Geo-Store ◄────────┘
      │
      ▼
Real-Time Ward Hazard Map & Safe Detour Routing
```

---

## 🚀 Dual-Track Hackathon Alignment

- 💻 **Build It Track (Local & Open-Source):** Runs 100% offline on localhost using SAM CLI, LocalStack (DynamoDB & Lambda), Strands Agents SDK, and open-source Cedar (`cedarpy`) for municipal disaster simulation drills.
- ☁️ **Ship It Track (Deployed Cloud URL):** Live deployment on AWS Amplify Hosting, Amazon API Gateway, AWS Lambda, Amazon DynamoDB, Amazon Cognito, and Amazon Bedrock.
