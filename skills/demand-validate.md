# Skill: demand-validate

> Validate market demand through data analysis and MVP testing
> 通过数据分析和 MVP 测试验证市场需求

## Definition

```yaml
skill:
  name: demand-validate
  version: 0.1.0
  description: "Verify that a sensed opportunity represents real, monetizable demand"
  trigger:
    - event: "opportunity_identified"
    - manual: true
  inputs:
    - opportunity_id: string
    - validation_methods: list[enum[landing_page, ad_test, survey, competitor_analysis, mvp]]
    - budget_limit: float
    - duration_days: int
  outputs:
    - validation_report: ValidationReport
    - go_no_go_decision: enum[proceed, pivot, archive]
    - refined_market_estimate: MarketEstimate
  dependencies:
    - market-sense
```

## Validation Methods / 验证方法

### 1. Smoke Test (Landing Page) / 烟雾测试（着陆页）
- Auto-generate landing page highlighting the value proposition
- Drive traffic via minimal ad spend
- Measure: email signups, click-through, time on page

### 2. Ad Campaign Test / 广告测试
- Run $50-$200 test campaigns on Google/Meta ads
- Measure CTR, CPC, and conversion intent signals

### 3. Survey / 调研
- Deploy AI-conducted user interviews via chat interface
- Survey target demographics about willingness to pay

### 4. Competitor Analysis / 竞品分析
- Analyze existing solutions' revenue, reviews, and market share
- Identify gaps in current offerings

### 5. MVP Test / MVP 测试
- Build minimal feature set
- Deploy to small user group
- Collect usage and satisfaction data

## Decision Matrix / 决策矩阵

```
PROCEED if:
  - willingness_to_pay > delivery_cost × 3
  - conversion_rate_mvp > 2%
  - competitor_count < 10 OR differentiation > 0.7

PIVOT if:
  - demand exists but wrong segment or pricing
  - technical feasibility issues

ARCHIVE if:
  - insufficient demand signals
  - market too small or declining
  - regulatory barriers too high
```
