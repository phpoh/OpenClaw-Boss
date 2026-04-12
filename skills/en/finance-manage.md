# 🦞 Skill: finance-manage

> **🇬🇧 English** | [🇨🇳 中文版](../cn/finance-manage.md)

---

## Metadata

| Field | Value |
|-------|-------|
| Name | `finance-manage` |
| Version | `1.0.0` |
| Owner | ⚡ Executor + 🪞 Reflector |
| Timeout | 3600s (1 hour, single execution) |
| Max Retries | 3 |
| Upstream Dependency | None (standalone) |

---

## Overview

The entrepreneur's "money management" ability. How much is coming in, how much is going out, should we spend it — OpenClaw watches the financial dashboard 24/7, auto-handles routine expenses, escalates big ones to humans.

---

## Strict Definition

```yaml
skill:
  name: finance-manage
  version: 1.0.0
  description: "Manage all financial aspects of the venture to ensure sustainability and growth"
  trigger:
    - scheduled: "0 9 * * *"
    - event: "revenue_received"
    - event: "payment_due"
    - event: "budget_threshold_reached"
    - event: "anomaly_detected"
    - manual: true
  inputs:
    - name: company_id
      type: "string"
      required: true
    - name: review_period
      type: "enum[daily, weekly, monthly, quarterly]"
      required: false
      default: "daily"
    - name: alert_config
      type: "AlertConfig"
      required: false
      description: "Custom alert configuration"
  outputs:
    - name: financial_report
      type: "FinancialReport"
      description: "Financial report"
    - name: cash_flow_projection
      type: "CashFlowProjection"
      description: "Cash flow projection (6 months)"
    - name: action_items
      type: "list[FinancialAction]"
      description: "Recommended financial actions"
  dependencies:
    skills: []
    tools:
      - bank_api
      - payment_processor
      - accounting_software
      - invoice_generator
      - tax_calculator
```

---

## Autonomous Spending Authority

```
┌──────────────────────┬────────────┬──────────────────────────────────┐
│ Amount Range         │ Action     │ Approval Required                │
├──────────────────────┼────────────┼──────────────────────────────────┤
│ < $100               │ ✅ Execute │ None, log only                   │
│ $100 - $1,000        │ 📢 Execute │ Notify human within 24h          │
│ $1,000 - $10,000     │ ⏸️ Queue   │ Human approval required          │
│ > $10,000            │ 🛑 Freeze  │ Human + board approval required  │
└──────────────────────┴────────────┴──────────────────────────────────┘

Special rules:
  - Same vendor cumulative spend > $700 in 7 days → escalate one approval level
  - Monthly spend > monthly budget × 1.1 → auto-freeze non-essential spending
  - Any cryptocurrency transaction → always requires human approval
```

---

## Financial Health Indicators

```
Daily checks:
  💵 Cash Balance     = bank_balance + payment_processor_balance
  🔥 Burn Rate        = monthly_expenses / months
  📅 Runway           = current_cash / burn_rate
  📊 Rev/Exp Ratio    = monthly_revenue / monthly_expenses

Healthy thresholds:
  ✅ Runway > 6 months
  ✅ Rev/Exp ratio > 0.5 (growth) or > 1.0 (mature)
  ✅ Gross margin > 60%
  ✅ CAC payback < 12 months
  ✅ LTV / CAC > 3

Alert rules:
  🟡 Runway < 6 months    → Suggest cost cuts or revenue acceleration
  🔴 Runway < 3 months    → Emergency notify human + activate emergency mode
  🔴 Daily anomaly > 3x avg → Freeze transaction + notify human
```

---

## Financial Report Template

```json
{
  "report_id": "fin_2026_0413_daily",
  "company_id": "comp_001",
  "period": "daily",
  "date": "2026-04-13",
  "summary": {
    "cash_balance": 125000.00,
    "daily_revenue": 3200.00,
    "daily_expenses": 1800.00,
    "net_daily": 1400.00,
    "mrr": 96000.00,
    "burn_rate": 72000.00,
    "runway_months": 8.7
  },
  "breakdown": {
    "revenue": {
      "subscriptions": 2800.00,
      "one_time": 400.00,
      "refunds": -0.00
    },
    "expenses": {
      "human_payroll": 1200.00,
      "ai_agent_costs": 150.00,
      "infrastructure": 200.00,
      "marketing": 180.00,
      "tools_and_subs": 70.00
    }
  },
  "action_items": [
    {
      "type": "optimization",
      "description": "AI agent API costs up 40% MoM, consider optimizing prompts or switching models",
      "priority": "medium"
    }
  ],
  "health_score": 0.82
}
```

---

## Error Handling

| Error Code | Description | Recovery Strategy |
|------------|-------------|-------------------|
| `PAYMENT_FAILED` | Payment execution failed | Retry 3x (1h/6h/24h intervals); still failing → notify human |
| `BANK_SYNC_ERROR` | Bank data sync failed | Use last successful snapshot + alert |
| `ANOMALOUS_TRANSACTION` | Anomalous transaction detected | Freeze transaction, notify human, await confirmation |
| `TAX_FILING_DUE` | Tax filing deadline | Generate filing materials 7 days early, 🛡️ human confirms before submit |
| `CASHFLOW_NEGATIVE` | Negative cash flow | Auto-generate cost reduction plan + 🛡️ human decision |

---

## Monitoring Metrics

```
finance_cash_balance_usd                        # Cash balance
finance_runway_months                           # Runway in months
finance_burn_rate_usd                           # Monthly burn rate
finance_revenue_expense_ratio                   # Revenue/expense ratio
finance_anomaly_detected_total                  # Anomaly detection count
finance_auto_approved_spend_usd                 # Auto-approved spend amount
finance_human_approval_pending_count            # Pending human approvals
```
