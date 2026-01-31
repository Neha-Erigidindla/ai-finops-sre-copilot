# System Architecture

## Overview

The AI FinOps & SRE Co-Pilot is a multi-agent system that optimizes cloud infrastructure costs while maintaining service-level objectives (SLOs). It demonstrates production-grade thinking by separating cost optimization (FinOps) from reliability validation (SRE), then orchestrating both to deliver safe, actionable recommendations.

## High-Level Architecture
```
┌─────────────────────────────────────────────────────────────┐
│                         USER                                │
│    "Analyze my cloud and optimize costs safely"            │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│              ORCHESTRATOR AGENT                             │
│         (FinOps & SRE Co-Pilot)                            │
│  • Manages workflow                                         │
│  • Delegates to specialist agents                           │
│  • Synthesizes final report                                 │
└──────────┬────────────────────────────┬─────────────────────┘
           │                            │
           ▼                            ▼
┌──────────────────────┐    ┌──────────────────────┐
│   FINOPS ANALYZER    │    │  SRE RISK EVALUATOR  │
│                      │    │                      │
│ • Analyzes CPU/mem   │    │ • Checks latency SLO │
│ • Finds waste        │    │ • Checks error SLO   │
│ • Calculates savings │    │ • Classifies risk    │
│                      │    │                      │
│ Output:              │    │ Output:              │
│ • Recommendations    │────│ • SAFE/CAUTION/RISKY │
│ • Savings estimates  │    │ • Deployment advice  │
└──────────┬───────────┘    └───────────┬──────────┘
           │                            │
           └────────────┬───────────────┘
                        │
                        ▼
        ┌───────────────────────────────┐
        │    KNOWLEDGE BASE (CSV)       │
        │  • Service metadata           │
        │  • Utilization metrics        │
        │  • Cost data                  │
        │  • SLO thresholds             │
        └───────────────────────────────┘
```

## Component Details

### 1. Orchestrator Agent (FinOps & SRE Co-Pilot)

**Responsibility:** Workflow coordination and executive reporting

**Workflow:**
```
1. Receive user query
2. Delegate to FinOps Analyzer
   ↓
3. Receive cost recommendations
4. Delegate to SRE Risk Evaluator (with FinOps output)
   ↓
5. Receive risk classifications
6. Synthesize final report
7. Return to user
```

**Key Features:**
- Multi-agent orchestration
- Context preservation across agent calls
- Executive-level report generation
- Risk-based categorization (SAFE/CAUTION/RISKY)

---

### 2. FinOps Analyzer Agent

**Responsibility:** Cost optimization analysis

**Analysis Logic:**
```python
for service in infrastructure:
    if cpu_utilization < 50% or memory_utilization < 50%:
        # Service is over-provisioned
        recommendation = downsize_instance(service)
        savings = calculate_savings(current_cost, new_instance_cost)
        
        return {
            "service": service.name,
            "current_cost": service.cost,
            "recommendation": recommendation,
            "savings": savings,
            "reasoning": "Low utilization indicates over-provisioning"
        }
```

**Output Example:**
```
Service: payment-api
Current: m5.2xlarge @ $450/month
Recommendation: Downsize to m5.xlarge
Savings: $180/month
Reasoning: 35% CPU, 40% memory - over-provisioned
```

---

### 3. SRE Risk Evaluator Agent

**Responsibility:** Reliability impact assessment

**Risk Classification Logic:**
```python
def classify_risk(service, recommendation):
    # Calculate SLO headroom
    latency_headroom = (slo_latency - current_latency) / slo_latency * 100
    error_headroom = (slo_error_rate - current_error_rate) / slo_error_rate * 100
    
    # Use worst-case metric
    min_headroom = min(latency_headroom, error_headroom)
    
    if min_headroom > 30:
        return "SAFE"
    elif 10 <= min_headroom <= 30:
        return "CAUTION"
    else:
        return "RISKY"
```

**Decision Matrix:**

| Headroom | Classification | Action |
|----------|---------------|---------|
| >30% | SAFE ✓ | Approve for deployment |
| 10-30% | CAUTION ⚠️ | Phased rollout with monitoring |
| <10% | RISKY 🚫 | Block change |

---

### 4. Knowledge Base (CSV Data)

**Schema:**
```
service_name          | Unique service identifier
instance_type         | AWS instance type (m5.xlarge, etc.)
vcpu                  | Virtual CPU count
memory_gb             | Memory in GB
cpu_util_percent      | Average CPU utilization (0-100%)
mem_util_percent      | Average memory utilization (0-100%)
monthly_cost_usd      | Current monthly cost
avg_latency_ms        | Average response latency
error_rate_percent    | Error rate percentage
slo_latency_ms        | Latency SLO threshold
slo_error_rate_percent| Error rate SLO threshold
```

## Design Decisions

### Why Multi-Agent Architecture?

**1. Separation of Concerns**
- FinOps and SRE have different priorities (cost vs reliability)
- Prevents conflicting logic in single agent
- Mirrors real-world team structures

**2. Modularity**
- Easy to test each agent independently
- Can replace/upgrade individual agents
- Simple to add new specialists (Security, Compliance, etc.)

**3. Clarity**
- Each agent has clear, focused instructions
- No ambiguity about responsibilities
- Easier to debug and improve

### Why SLO-Based Risk Classification?

**1. Industry Standard**
- Google SRE practices use SLO budgets
- Quantifiable reliability metrics
- Clear success/failure criteria

**2. Automated Decision Making**
- No subjective judgment required
- Consistent risk assessment
- Defensible recommendations

**3. Production Safety**
- Prevents changes that could breach SLOs
- Builds confidence in automation
- Reduces incident risk

## Key Insights

💡 Multi-agent systems require more upfront design but are easier to maintain  
💡 Production-ready thinking (SLOs, phased rollouts) demonstrates maturity  
💡 Quantifiable impact ($826 savings) is more compelling than vague "efficiency"  

---

**Architecture Version:** 1.0  
**Last Updated:** January 31, 2026
