# AI FinOps & SRE Co-Pilot

> **IBM Dev Day AI Demystified Hackathon 2026**

An intelligent multi-agent system that optimizes cloud infrastructure costs while maintaining service-level objectives (SLOs). Built using IBM watsonx Orchestrate.

## 🎯 Problem Statement

Cloud costs are rising 20-30% annually, but organizations struggle to optimize because:
- Aggressive cost-cutting can break production systems
- Manual analysis is slow and error-prone
- Trade-offs between cost and reliability are unclear
- Engineers lack confidence in infrastructure changes

**The cost of getting this wrong:** Production outages, SLO breaches, customer impact.

## 💡 Solution

A multi-agent AI system that thinks like real cloud teams at Google and Amazon:

### **Agent 1: FinOps Analyzer**
- Analyzes resource utilization (CPU, memory)
- Identifies over-provisioned instances
- Recommends specific downsizing actions
- Calculates potential savings

### **Agent 2: SRE Risk Evaluator**
- Reviews cost optimization recommendations
- Analyzes latency and error rate metrics
- Classifies changes: SAFE / CAUTION / RISKY
- Prevents SLO violations

### **Agent 3: Orchestrator (FinOps & SRE Co-Pilot)**
- Coordinates both specialized agents
- Synthesizes insights into executive reports
- Provides clear, actionable next steps
- Balances cost efficiency with reliability

## 🏗️ Architecture
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

**Key Design Principles:**
- **Separation of concerns** - Each agent has a focused responsibility
- **SLO-driven decisions** - Reliability metrics guide every recommendation
- **Multi-agent orchestration** - Agents collaborate to solve complex problems
- **Production-ready thinking** - Canary deployments, phased rollouts, risk management

## 📊 Results

**Analyzed Environment:**
- 10 cloud services (payment APIs, analytics, auth, dashboards)
- Mixed utilization patterns (20-90% CPU/memory)
- Varied SLO requirements (latency, error rates)

**System Output:**

| Category | Services | Monthly Savings | Status |
|----------|----------|----------------|--------|
| **SAFE** | 3 services | $626/month | ✅ Deploy immediately |
| **CAUTION** | 2 services | $200/month | ⚠️ Phased rollout with monitoring |
| **RISKY** | 1 service | $0 (blocked) | 🚫 Prevented potential outage |

**Total Potential Savings:** $826/month (40% cost reduction)

**Key Win:** System automatically blocked a risky change to `auth-service` that was operating at 94% of latency SLO - preventing a potential production incident.

## 🚀 How It Works

### User Query
```
"Analyze my cloud infrastructure and recommend safe cost optimizations"
```

### System Response
```
📊 EXECUTIVE SUMMARY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Total Potential Monthly Savings: $826/month

- Safe changes: 3 services ($626/month)
- Cautionary changes: 2 services ($200/month)
- Blocked changes: 1 service ($0/month)

Overall Recommendation: Deploy 3 safe optimizations immediately.

✅ SAFE RECOMMENDATIONS

Service: payment-api
Current: m5.2xlarge @ $450/month
Proposed: m5.xlarge @ $270/month
Savings: $180/month
SLO Safety:
  • Latency: 45ms (SLO: 100ms) → 55% headroom ✓
  • Error Rate: 0.1% (SLO: 0.5%) → 80% headroom ✓
Action: Deploy via standard change process

🚫 BLOCKED RECOMMENDATIONS

Service: auth-service
Why Blocked: Latency headroom only 6% - operating at edge of SLO
Alternative: Optimize performance before downsizing
Action: Do not implement
```

## 🛠️ Technical Stack

- **Platform:** IBM watsonx Orchestrate
- **AI Model:** IBM Granite 3.1 8B Instruct
- **Framework:** Multi-agent orchestration
- **Data:** Mock cloud infrastructure metrics (CSV)
- **Concepts:** FinOps, SRE, SLO management, risk classification

## 📁 Repository Structure
```
ai-finops-sre-copilot/
├── README.md                          # You are here
├── docs/
│   ├── architecture.md                # Detailed system design
│   └── agent-instructions.md          # Complete agent prompts
├── data/
│   └── cloud_infrastructure_metrics.csv  # Sample infrastructure data
└── screenshots/
    └── [Demo screenshots]
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
- GitHub: [Neha-Erigidindla](https://github.com/Neha-Erigidindla/ai-finops-sre-copilot)
- Hackathon: IBM Dev Day AI Demystified 2026

## 🙏 Acknowledgments

- IBM watsonx team for the orchestration platform
- IBM Granite model team for high-quality inference
- Cloud FinOps Foundation for cost optimization frameworks
- Google SRE book for reliability engineering principles

---

**Built for the IBM Dev Day AI Demystified Hackathon**
