# Skill: finance-manage

> Manage revenue, expenses, payroll, and reinvestment
> 管理收入、支出、薪资和再投资

## Definition

```yaml
skill:
  name: finance-manage
  version: 0.1.0
  description: "Manage all financial aspects of the venture to ensure sustainability and growth"
  trigger:
    - scheduled: "0 9 * * *"    # Daily financial review
    - event: "revenue_received"
    - event: "payment_due"
    - event: "budget_threshold_reached"
    - manual: true
  inputs:
    - company_id: string
    - financial_period: enum[daily, weekly, monthly, quarterly]
  outputs:
    - financial_report: FinancialReport
    - cash_flow_projection: CashFlowProjection
    - budget_recommendations: Recommendation[]
  dependencies: []
```

## Financial Operations / 财务运营

### Revenue Management / 收入管理
- Track all income sources in real-time
- Reconcile payment processor payouts
- Monitor subscription metrics (MRR, churn, LTV)
- Forecast revenue based on growth trends

### Expense Management / 支出管理
- Categorize and track all expenses
- Manage vendor payments and subscriptions
- Optimize cloud and infrastructure costs
- Flag unusual spending patterns

### Payroll / 薪资
- Process payments to human team members
- Handle tax withholdings where applicable
- Manage contractor payments via platforms
- Track AI agent costs (API usage, compute)

### Reporting / 报告
- Daily cash position
- Weekly P&L summary
- Monthly financial statements
- Quarterly investor reports (if applicable)

## Autonomy Tiers / 自主等级

| Amount / 金额 | Action / 行为 | Approval / 审批 |
|---------------|--------------|----------------|
| < $100 / ¥700 | Auto-execute | None |
| $100 - $1,000 / ¥700-7,000 | Auto-execute | Notify human |
| $1,000 - $10,000 / ¥7,000-70,000 | Queue | Human approval |
| > $10,000 / ¥70,000+ | Queue | Human + board review |

## Key Metrics / 关键指标

```
burn_rate = monthly_expenses / months
runway = current_cash / burn_rate
unit_economics = LTV / CAC
break_even_point = fixed_costs / (price_per_unit - variable_cost_per_unit)

Healthy thresholds:
  - runway > 6 months
  - LTV / CAC > 3
  - gross_margin > 60%
```
