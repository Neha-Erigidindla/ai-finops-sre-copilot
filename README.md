# 🚀 AI FinOps & SRE Co-Pilot  
> **IBM Dev Day – AI Demystified Hackathon 2026**

🔍 *Optimize cloud costs without breaking production.*

An **agentic AI system** that analyzes cloud infrastructure **cost + performance metrics** and recommends **safe, SLO-aware optimizations** — inspired by real **FinOps & SRE practices at Google and Amazon**.

Built using **IBM watsonx Orchestrate** and **IBM Granite 3.1**.

---

## 🎯 Problem Statement

Cloud costs are increasing **20–30% every year**, but organizations struggle to optimize because:

- ❌ Aggressive cost-cutting breaks production systems  
- ❌ Manual analysis is slow and error-prone  
- ❌ Cost vs reliability trade-offs are unclear  
- ❌ Engineers fear SLO violations  

💥 **The real risk:** outages, SLO breaches, customer impact, and revenue loss.

---

## 💡 Solution Overview

A **multi-agent AI system** that behaves like a real cloud operations team.

### 🟦 FinOps Analyzer Agent
- Analyzes CPU & memory utilization
- Detects over-provisioned resources
- Recommends right-sizing actions
- Estimates monthly cost savings

### 🟩 SRE Risk Evaluator Agent
- Reviews FinOps recommendations
- Analyzes latency & error-rate SLOs
- Classifies changes as:
  - ✅ SAFE  
  - ⚠️ CAUTION  
  - 🚫 RISKY  
- Blocks changes that could break production

### 🟨 Orchestrator (FinOps & SRE Co-Pilot)
- Coordinates all agents
- Synthesizes insights into executive-level reports
- Balances **cost efficiency + reliability**
- Produces clear, actionable decisions

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────┐
│  User: "Optimize my cloud infrastructure"  │
└───────────────────┬─────────────────────────┘
                    │
             ┌──────▼──────────┐
             │  ORCHESTRATOR   │
             │  (Granite 3.1)  │
             └──┬───────────┬──┘
                │           │
         ┌──────▼─────┐  ┌─▼──────────┐
         │   FinOps   │  │    SRE     │
         │  Analyzer  │  │    Risk    │
         │            │  │ Evaluator  │
         └──────┬─────┘  └─────┬──────┘
                │              │
                └──────┬───────┘
                       │
              ┌────────▼─────────┐
              │  Executive Report│
              │  • Safe: $626/mo │
              │  • Caution: $200 │
              │  • Blocked: 1    │
              └──────────────────┘
```

---

## 📸 Demo Screenshots

### 🔹 Multi-Agent Configuration
![Three Agents](screenshots/01-three-agents.png)

### 🔹 Orchestrator ↔ Agent Conversation
![Conversation](screenshots/02-orchestrator-conversation.png)
![Conversation 1](screenshots/02-orchestrator-conversation_1.png)
![Conversation 2](screenshots/02-orchestrator-conversation_2.png)

### 🔹 Executive Summary Output
![Executive Summary](screenshots/03-executive-summary.png)

### 🔹 Safe Cost Optimizations
![Safe Recommendations](screenshots/04-safe-recommendations.png)

### 🔹 Blocked Risky Change
![Blocked Risky Change](screenshots/05-blocked-risky-change.png)

---

## 📊 Results

**Analyzed Environment**
- 10 cloud services (auth, payment, analytics, dashboards)
- Mixed utilization (20–90%)
- Strict latency & error SLOs

| Category | Services | Monthly Savings | Status |
|--------|----------|-----------------|--------|
| ✅ SAFE | 3 | $626/month | Deploy immediately |
| ⚠️ CAUTION | 2 | $200/month | Canary rollout |
| 🚫 RISKY | 1 | $0 | Blocked |

💡 **Total Potential Savings:** **$826/month (~40%)**

🚨 **Key Win:**  
The system automatically **blocked a risky downsizing** of `auth-service` operating at **94% of latency SLO**, preventing a potential production outage.

---

## 🧠 Example System Output

```text
EXECUTIVE SUMMARY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Total Monthly Savings: $826

Safe changes: 3 services ($626)
Caution changes: 2 services ($200)
Blocked changes: 1 service

Recommendation:
Deploy safe optimizations immediately.
Use phased canary deployments for caution items.

## 🛠️ Technical Stack

- **Platform:** IBM watsonx Orchestrate
- **AI Model:** IBM Granite 3.1 8B Instruct
- **Framework:** Multi-agent orchestration
- **Data:** Mock cloud infrastructure metrics (CSV)
- **Concepts:** FinOps, SRE, SLO management, risk classification

## 📁 Repository Structure
```
ai-finops-sre-copilot/
├── README.md
├── data/
│   └── cloud_infrastructure_metrics.csv
├── agents/
│   ├── finops_analyzer.prompt
│   ├── sre_risk_evaluator.prompt
│   └── orchestrator.prompt
└── screenshots/
    ├── 01-three-agents.png
    ├── 02-orchestrator-conversation.png
    ├── 02-orchestrator-conversation_1.png
    ├── 02-orchestrator-conversation_2.png
    ├── 03-executive-summary.png
    ├── 04-safe-recommendations.png
    └── 05-blocked-risky-change.png

```

## 🎓 What I Learned

Building this system reinforced key concepts that define modern cloud operations:

### **Multi-Agent Systems**
- Separation of concerns improves reliability
- Agent orchestration enables complex workflows
- Specialized agents > general-purpose prompts

### **FinOps Principles**
- Utilization monitoring identifies waste
- Right-sizing requires domain knowledge
- Cost optimization is continuous, not one-time

### **SRE Best Practices**
- SLOs define acceptable risk boundaries
- Headroom analysis prevents incidents
- Phased rollouts reduce blast radius
- Automated safety checks prevent human error

## 🌟 Real-World Applications

This pattern can be extended to:

- **Kubernetes cluster optimization** - Right-size pods, optimize resource requests
- **Database cost management** - Balance IOPS, storage, and query performance
- **Auto-scaling optimization** - Tune scaling policies against traffic patterns
- **Multi-cloud cost governance** - Compare pricing across AWS, Azure, GCP

## 🏆 Hackathon Impact

This project demonstrates:

✅ **Technical depth** - Multi-agent AI orchestration  
✅ **Business value** - $826/month savings for a 10-service environment  
✅ **Production readiness** - SLO monitoring, risk classification, deployment strategies  
✅ **Real-world thinking** - Mirrors how Google/Amazon manage infrastructure  

**Theme alignment:** "AI Demystified - From idea to deployment"
- Shows how approachable AI can simplify complex cloud operations
- Demonstrates practical AI that teams can deploy immediately
- Proves AI can make technical decisions confidently and safely

## 👨‍💻 Author
- GitHub: [Neha-Erigidindla](https://github.com/Neha-Erigidindla)
- Hackathon: IBM Dev Day AI Demystified 2026

## 🙏 Acknowledgments

- IBM watsonx team for the orchestration platform
- IBM Granite model team for high-quality inference
- Cloud FinOps Foundation for cost optimization frameworks
- Google SRE book for reliability engineering principles

---

**Built for the IBM Dev Day AI Demystified Hackathon**
