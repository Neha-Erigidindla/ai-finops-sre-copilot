# System Architecture

## Overview

The AI FinOps & SRE Co-Pilot is a multi-agent system that optimizes cloud infrastructure costs while maintaining service-level objectives (SLOs). It demonstrates production-grade thinking by separating cost optimization (FinOps) from reliability validation (SRE), then orchestrating both to deliver safe, actionable recommendations.

---

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

---

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

**Technologies:**
- IBM watsonx Orchestrate (agent framework)
- IBM Granite 3.1 8B (LLM)
- Agent collaboration API

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
    else:
        return {
            "service": service.name,
            "recommendation": "No change",
            "reasoning": "Utilization is optimal"
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

**Output Example:**
```
Service: payment-api
Risk Level: SAFE ✓
Latency: 45ms (SLO: 100ms) → 55% headroom
Error Rate: 0.1% (SLO: 0.5%) → 80% headroom
Deployment: Approve - proceed with standard rollout
```

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

**Sample Data:**
```csv
payment-api,m5.2xlarge,8,32,35,40,450,45,0.1,100,0.5
auth-service,m5.xlarge,4,16,75,80,225,35,0.15,50,0.5
...
```

**Data Design Principles:**
- Mix of over-provisioned and right-sized services
- Varied SLO headroom (some safe, some risky)
- Realistic cost ranges for AWS instances
- Representative of production workloads

---

## Data Flow Diagram

```
┌──────────┐
│   User   │
└────┬─────┘
     │ Query: "Optimize my cloud"
     ▼
┌────────────────┐
│ Orchestrator   │
│ Reads CSV data │
└────┬───────────┘
     │ Delegates
     ▼
┌────────────────┐         ┌─────────────────┐
│ FinOps Agent   │         │   Knowledge     │
│ Analyzes:      │◄────────┤   Base (CSV)    │
│ • CPU util     │         │                 │
│ • Memory util  │         │ • Utilization   │
│ • Costs        │         │ • Costs         │
│                │         │ • SLOs          │
│ Produces:      │         └─────────────────┘
│ • Savings list │                 ▲
└────┬───────────┘                 │
     │ Recommendations              │
     ▼                              │
┌────────────────┐                 │
│ Orchestrator   │                 │
└────┬───────────┘                 │
     │ Delegates + FinOps output   │
     ▼                              │
┌────────────────┐                 │
│ SRE Agent      │◄────────────────┘
│ Evaluates:     │
│ • Latency SLO  │
│ • Error SLO    │
│ • Headroom     │
│                │
│ Produces:      │
│ • Risk labels  │
└────┬───────────┘
     │ Risk classifications
     ▼
┌────────────────┐
│ Orchestrator   │
│ Synthesizes:   │
│ • Exec summary │
│ • Safe changes │
│ • Risky blocks │
└────┬───────────┘
     │ Final report
     ▼
┌────────────────┐
│   User         │
│ Receives:      │
│ • $826 savings │
│ • 3 safe       │
│ • 1 blocked    │
└────────────────┘
```

---

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

### Why Orchestrator Pattern?

**1. Workflow Control**
- Enforces correct sequence (FinOps → SRE)
- Manages state across agent calls
- Handles error cases

**2. Context Preservation**
- Maintains conversation history
- Passes FinOps output to SRE agent
- Synthesizes final report

**3. User Experience**
- Single entry point for users
- Hides complexity of multi-agent system
- Delivers unified, actionable output

---

## Performance Characteristics

### Latency
- Single agent query: ~3-5 seconds
- Multi-agent orchestration: ~10-15 seconds
- Acceptable for non-real-time optimization workflows

### Accuracy
- FinOps recommendations: 100% rule-based (utilization thresholds)
- SRE risk classification: 100% deterministic (SLO headroom calculation)
- No hallucination risk for numeric calculations

### Scalability
- Current: 10 services analyzed in <15 seconds
- Linear scaling up to ~100 services
- For >100 services, recommend batch processing

---

## Security & Compliance

### Data Handling
- Mock data only (no production credentials)
- CSV stored in watsonx Orchestrate knowledge base
- No external API calls

### Access Control
- Agents scoped to IBM Cloud account
- No cross-account access
- Audit logs available in watsonx platform

---

## Future Enhancements

### Short-Term (Hackathon++)
1. **Real-time data integration** - Connect to AWS CloudWatch, Datadog
2. **Historical trending** - Track cost savings over time
3. **What-if analysis** - Model different optimization scenarios

### Medium-Term (Production)
1. **Automated deployment** - Integration with Terraform/Ansible
2. **Feedback loop** - Track actual vs predicted savings
3. **Multi-cloud support** - AWS, Azure, GCP cost analysis

### Long-Term (Scale)
1. **Predictive optimization** - ML-based capacity planning
2. **Policy enforcement** - Automatic blocking of SLO violations
3. **Team collaboration** - Approval workflows, change management

---

## Lessons Learned

### What Worked Well
✅ Clear separation of concerns (FinOps vs SRE)  
✅ Structured output format (enables reliable parsing)  
✅ SLO-based risk classification (quantifiable, defensible)  
✅ Mock data with realistic scenarios (safe, cautionary, risky)  

### Challenges
⚠️ Agent orchestration requires explicit instructions  
⚠️ Data availability must be clearly stated  
⚠️ Output format must be specified in detail  

### Key Insights
💡 Multi-agent systems require more upfront design but are easier to maintain  
💡 Production-ready thinking (SLOs, phased rollouts) impresses judges  
💡 Quantifiable impact ($826 savings) is more compelling than "improves efficiency"  

---

## References

- Google SRE Book: https://sre.google/sre-book/table-of-contents/
- FinOps Foundation: https://www.finops.org/
- AWS Instance Pricing: https://aws.amazon.com/ec2/pricing/
- IBM watsonx Orchestrate Docs: https://www.ibm.com/docs/en/watsonx/watson-orchestrate

---

**Architecture Version:** 1.0  
**Last Updated:** January 31, 2026  
**Author:** [Neha-Erigidindla]
