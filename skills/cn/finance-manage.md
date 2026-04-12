# 🦞 Skill: finance-manage（财务管理）

> **🇨🇳 中文版** | [🇬🇧 English](../en/finance-manage.md)

---

## 元信息

| 字段 | 值 |
|------|-----|
| 名称 | `finance-manage` |
| 版本 | `1.0.0` |
| 负责组件 | ⚡ 执行器（Executor）+ 🪞 反思器（Reflector） |
| 超时限制 | 3600 秒（1 小时，单次执行） |
| 最大重试次数 | 3 |
| 上游依赖 | 无（独立运行） |

---

## 概述

企业家的"管钱"能力。赚多少、花多少、该不该花——OpenClaw 24/7 盯着财务仪表盘，自动处理日常开支，大额支出找人类拍板。

---

## 严格定义

```yaml
skill:
  name: finance-manage
  version: 1.0.0
  description: "管理企业所有财务方面，确保可持续性和增长"
  trigger:
    - scheduled: "0 9 * * *"              # 每日 9:00 财务审查
    - event: "revenue_received"            # 收到收入
    - event: "payment_due"                 # 应付款到期
    - event: "budget_threshold_reached"    # 预算达到阈值
    - event: "anomaly_detected"            # 异常交易检测
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
      description: "自定义告警配置"
  outputs:
    - name: financial_report
      type: "FinancialReport"
      description: "财务报告"
    - name: cash_flow_projection
      type: "CashFlowProjection"
      description: "现金流预测（6 个月）"
    - name: action_items
      type: "list[FinancialAction]"
      description: "建议采取的财务行动"
  dependencies:
    skills: []
    tools:
      - bank_api                # 银行账户 API
      - payment_processor       # 支付处理（Stripe / 支付宝）
      - accounting_software     # 记账软件 API
      - invoice_generator       # 发票生成
      - tax_calculator          # 税务计算
```

---

## 自主支出权限矩阵

```
┌──────────────────────┬────────────┬──────────────────────────────────┐
│ 金额范围              │ 行为       │ 审批要求                          │
├──────────────────────┼────────────┼──────────────────────────────────┤
│ < ¥700 / $100        │ ✅ 直接执行 │ 无需审批，记录日志                │
│ ¥700-7,000 / $100-1K │ 📢 执行    │ 执行后 24h 内通知人类             │
│ ¥7K-70K / $1K-10K    │ ⏸️ 等待    │ 需人类审批后方可执行              │
│ > ¥70K / $10K        │ 🛑 冻结    │ 需人类 + 董事会双重审批          │
└──────────────────────┴────────────┴──────────────────────────────────┘

特殊规则：
  - 同一供应商 7 天内累计支出 > ¥5,000 / $700 → 升级一级审批
  - 月度支出 > 月度预算 × 1.1 → 自动冻结非必要支出
  - 任何加密货币相关交易 → 一律需要人类审批
```

---

## 财务健康指标

```
每日必检：
  💵 现金余额        = bank_balance + payment_processor_balance
  🔥 燃烧率          = monthly_expenses / months
  📅 资金跑道        = current_cash / burn_rate
  📊 收入/支出比     = monthly_revenue / monthly_expenses

健康阈值：
  ✅ 资金跑道 > 6 个月
  ✅ 收入/支出比 > 0.5（增长期）或 > 1.0（成熟期）
  ✅ 毛利率 > 60%
  ✅ 客户获取成本 (CAC) 回收周期 < 12 个月
  ✅ LTV / CAC > 3

告警规则：
  🟡 资金跑道 < 6 个月     → 建议削减开支或加速营收
  🔴 资金跑道 < 3 个月     → 紧急通知人类 + 启动 emergency 模式
  🔴 单日异常支出 > 日均 3x → 冻结该笔交易 + 通知人类
```

---

## 财务报告模板

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
      "description": "AI agent API 费用较上月增长 40%，建议优化 prompt 或切换模型",
      "priority": "medium"
    }
  ],
  "health_score": 0.82
}
```

---

## 错误处理

| 错误码 | 描述 | 处理策略 |
|--------|------|---------|
| `PAYMENT_FAILED` | 支付执行失败 | 重试 3 次（间隔 1h/6h/24h）；仍失败通知人类 |
| `BANK_SYNC_ERROR` | 银行数据同步失败 | 使用最近一次成功快照 + 告警 |
| `ANOMALOUS_TRANSACTION` | 检测到异常交易 | 冻结交易，通知人类，等待确认 |
| `TAX_FILING_DUE` | 税务申报到期 | 提前 7 天生成申报材料，🛑 人类确认后提交 |
| `CASHFLOW_NEGATIVE` | 现金流为负 | 自动生成削减方案 + 🛡️ 人类决策 |

---

## 监控指标

```
finance_cash_balance_usd                        # 现金余额
finance_runway_months                           # 资金跑道月数
finance_burn_rate_usd                           # 月度燃烧率
finance_revenue_expense_ratio                   # 收入支出比
finance_anomaly_detected_total                  # 异常交易检测次数
finance_auto_approved_spend_usd                 # 自动审批支出额
finance_human_approval_pending_count            # 待人类审批数量
```
