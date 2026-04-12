# 🦞 Skill: growth-scale

> **🇬🇧 English** | [🇨🇳 中文版](../cn/growth-scale.md)

---

## Metadata

| Field | Value |
|-------|-------|
| Name | `growth-scale` |
| Version | `1.0.0` |
| Owner | 🧭 Planner + 🪞 Reflector |
| Timeout | 3600s (1 hour, single review) |
| Max Retries | 1 |
| Upstream Dependency | `finance-manage` |

---

## Overview

The entrepreneur's ability to "scale up." Product is live, money is coming in — what's next? OpenClaw decides whether to invest in growth, hold steady, or gracefully exit. Manages a portfolio of ventures like a true investor-boss.

---

## Strict Definition

```yaml
skill:
  name: growth-scale
  version: 1.0.0
  description: "Scale successful ventures, manage portfolio, and gracefully exit failures"
  trigger:
    - scheduled: "0 9 * * 1"
    - event: "growth_milestone_reached"
    - event: "decline_detected"
    - event: "runway_below_threshold"
    - manual: true
  inputs:
    - name: company_id
      type: "string"
      required: true
    - name: performance_data
      type: "PerformanceData"
      required: true
      description: "Current performance data (from finance-manage and product-build)"
    - name: market_conditions
      type: "MarketData"
      required: false
      description: "Current market environment data"
  outputs:
    - name: growth_plan
      type: "GrowthPlan"
      description: "Growth plan"
    - name: venture_status
      type: "enum[grow, hold, review, exit]"
      description: "Venture status decision"
    - name: strategic_recommendations
      type: "list[Recommendation]"
      description: "Strategic recommendations"
  dependencies:
    skills: ["finance-manage"]
    tools:
      - analytics_engine
      - ab_test_platform
      - marketing_automation
      - market_data_feed
```

---

## Venture Status Decision Matrix

```
┌──────────┬───────────────────────────────────────────────────┐
│ 📈 GROW  │ Revenue growth > 10% MoM                         │
│          │ AND LTV/CAC > 3                                   │
│          │ AND runway > 6 months                             │
│          │ → Increase investment, expand team, enter markets │
├──────────┼───────────────────────────────────────────────────┤
│ ⏸️ HOLD  │ Revenue growth 0-10% MoM                         │
│          │ AND LTV/CAC > 1.5                                 │
│          │ AND runway > 6 months                             │
│          │ → Maintain, optimize efficiency, find growth levers│
├──────────┼───────────────────────────────────────────────────┤
│ 🔍 REVIEW│ 2 consecutive months of negative growth           │
│          │ OR LTV/CAC < 1.5                                  │
│          │ OR runway < 6 months                              │
│          │ → Deep diagnosis, develop pivot plan              │
├──────────┼───────────────────────────────────────────────────┤
│ 🚪 EXIT  │ 3 consecutive months: burn > revenue × 2          │
│          │ AND customer growth < 5% MoM                      │
│          │ AND no path to profitability within 6 months      │
│          │ → Initiate graceful exit process                  │
└──────────┴───────────────────────────────────────────────────┘
```

---

## Growth Strategy Toolbox

### 🎯 Optimize

```
Auto-executable:
  [ ] A/B test pricing (3 price points)
  [ ] Optimize landing page conversion (headline, CTA, colors)
  [ ] Email marketing automation flow optimization
  [ ] Customer success process standardization
  [ ] Automate operational processes (reduce labor cost)

Metrics:
  - Conversion rate improvement
  - CAC reduction
  - Churn rate reduction
```

### 🌍 Expand

```
Requires planning + 🛡️ human approval:
  [ ] New customer segment targeting
  [ ] Adjacent market entry assessment
  [ ] Geographic expansion feasibility
  [ ] Product line extension planning
  [ ] Partnership/channel development

Each expansion requires:
  1. Market research report (auto-generated)
  2. ROI projection
  3. Risk assessment
  4. 🛡️ Human approval
```

### ⚙️ Scale

```
  [ ] Infrastructure scaling (auto elastic)
  [ ] Team expansion (trigger team-assemble)
  [ ] Process standardization (SOP docs)
  [ ] Customer support system upgrade
  [ ] Compliance framework enhancement
```

### 🔄 Diversify

```
Leverage existing infrastructure for new ventures:
  [ ] Identify reusable tech/team/customer assets
  [ ] Generate new venture opportunity assessment
  [ ] Trigger market-sense → demand-validate full pipeline
  [ ] 🛡️ Human approval to launch new venture
```

---

## Graceful Exit Protocol

```
Exit process (estimated 2-4 weeks):

Week 1: Notification & Assessment
  [ ] Notify all stakeholders (human team, customers, vendors)
  [ ] Assess outstanding contracts and commitments
  [ ] Generate exit cost estimate

Week 2: Orderly Wind-down
  [ ] Complete all outstanding customer deliverables
  [ ] Settle all outstanding payments
  [ ] Notify AI workers to stop

Week 3: Asset Disposition
  [ ] Archive code, data, and documentation
  [ ] Extract lessons learned → write to Memory
  [ ] Reassign human team members
  [ ] Assess asset sale possibilities

Week 4: Final Liquidation
  [ ] Close legal entity
  [ ] Final financial reconciliation
  [ ] Tax clearance
  [ ] Generate post-mortem report
```

---

## Portfolio Management

```
┌─────────────────────────────────────────────────────────────┐
│                  🦞 OpenClaw-Boss Portfolio                  │
├──────────────┬──────────┬──────────┬──────────┬─────────────┤
│ Venture      │ Status   │ Rev/Mo   │ Growth   │ Decision    │
├──────────────┼──────────┼──────────┼──────────┼─────────────┤
│ A (SaaS)     │ 📈 Grow  │ $32,000  │ +15% MoM │ Invest more │
│ B (API)      │ ⏸️ Hold  │ $12,000  │ +3% MoM  │ Optimize    │
│ C (App)      │ 🔍 Review│ $4,000   │ -5% MoM  │ Diagnose    │
│ D (Shop)     │ 🚪 Exit  │ $800     │ -20% MoM │ Wind down   │
├──────────────┼──────────┼──────────┼──────────┼─────────────┤
│ Total        │          │ $48,800  │ +8% MoM  │             │
│ Burn Rate    │          │ $38,000  │          │             │
│ Runway       │          │ 7.2 mo   │          │             │
└──────────────┴──────────┴──────────┴──────────┴─────────────┘

Resource allocation rules:
  - GROW ventures get 60% of new resources
  - HOLD ventures get 25% of new resources
  - REVIEW ventures get 15% of new resources
  - EXIT ventures get 0% new resources, gradually release
```

---

## Error Handling

| Error Code | Description | Recovery Strategy |
|------------|-------------|-------------------|
| `MARKET_CRASH` | Severe market disruption | Freeze all expansion plans immediately, notify human |
| `COMPETITOR_DISRUPTION` | Major competitor launch | Trigger emergency market-sense deep scan |
| `KEY_PERSON_LEFT` | Key personnel departure | Trigger team-assemble emergency hiring |
| `REGULATION_CHANGE` | Regulatory change | 🛡️ Pause operations, human assesses compliance impact |
| `DATA_BREACH` | Data breach detected | Isolate affected systems immediately, notify human, start incident response |

---

## Monitoring Metrics

```
growth_scale_portfolio_revenue_usd             # Total portfolio revenue
growth_scale_venture_count_by_status           # Venture count by status
growth_scale_exit_success_rate                 # Exit success rate
growth_scale_resource_allocation_efficiency    # Resource allocation efficiency
growth_scale_time_to_growth_decision_seconds   # Growth decision latency
```
