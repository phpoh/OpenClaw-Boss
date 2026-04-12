# 🦞 Skill: demand-validate

> **🇬🇧 English** | [🇨🇳 中文版](../cn/demand-validate.md)

---

## Metadata

| Field | Value |
|-------|-------|
| Name | `demand-validate` |
| Version | `1.0.0` |
| Owner | 🧭 Planner |
| Executor | ⚡ Executor |
| Timeout | 7200s (2 hours) |
| Max Retries | 2 |
| Upstream Dependency | `market-sense` |

---

## Overview

The entrepreneur's "judgment." After discovering an opportunity, don't rush in — validate first: Is the demand real? How much will people pay? How good are the competitors? Maximize certainty at minimum cost.

---

## Strict Definition

```yaml
skill:
  name: demand-validate
  version: 1.0.0
  description: "Validate whether a market opportunity represents real, monetizable demand at minimal cost"
  trigger:
    - event: "opportunity_identified"
    - event: "opportunity_score_above_0.6"
    - manual: true
  inputs:
    - name: opportunity_id
      type: "string"
      required: true
      description: "Opportunity ID from market-sense output"
    - name: validation_methods
      type: "list[enum[landing_page, ad_test, survey, competitor_analysis, mvp]]"
      required: false
      default: ["landing_page", "competitor_analysis", "survey"]
      description: "Validation methods to execute"
    - name: budget_limit
      type: "float"
      required: true
      constraints: "value >= 0"
      description: "Budget cap for validation phase (USD)"
    - name: duration_days
      type: "int"
      required: false
      default: 7
      constraints: "1 <= value <= 30"
      description: "Validation period in days"
  outputs:
    - name: validation_report
      type: "ValidationReport"
      description: "Complete validation report"
    - name: decision
      type: "enum[proceed, pivot, archive]"
      description: "Go/No-Go decision"
    - name: refined_estimate
      type: "MarketEstimate"
      description: "Refined market estimate after validation"
  dependencies:
    skills: ["market-sense"]
    tools:
      - web_search
      - landing_page_builder
      - ad_platform_api
      - survey_engine
      - payment_analytics
```

---

## Pre-conditions

```
PRE 1: opportunity_id exists in Memory with status active
PRE 2: budget_limit > 0
PRE 3: No in-progress validation task for the same opportunity_id
```

## Post-conditions

```
POST 1: Validation report includes results from all specified methods
POST 2: decision field is non-null
POST 3: Budget consumed <= budget_limit
POST 4: Results written to Memory, linked to original opportunity_id
```

---

## Validation Methods

### 1. 🎯 Landing Page Smoke Test

```
Flow:
  1. AI auto-generates landing page (headline + value prop + email signup)
  2. Deploy to Vercel/Cloudflare Pages (< 2 minutes)
  3. Spend $20-$50 on ads to drive traffic
  4. Collect data for 3-7 days

Metrics:
  - Page views (PV)
  - Email signup conversion rate
  - Average time on page
  - Bounce rate

Pass criteria:
  ✅ Signup conversion rate > 3%
  ✅ Avg time on page > 30s
  ✅ Bounce rate < 70%
```

### 2. 📢 Ad Campaign Test

```
Flow:
  1. AI generates 3 ad copy variants and creative
  2. Create test campaigns on Google Ads / Meta Ads
  3. Budget $30-$70 per variant, total $100-$200
  4. Run for 3-5 days

Metrics:
  - Click-through rate (CTR)
  - Cost per click (CPC)
  - Conversion intent signals

Pass criteria:
  ✅ CTR > 1.5%
  ✅ CPC < industry average × 1.2
```

### 3. 📋 User Survey

```
Flow:
  1. AI generates survey questionnaire (5-10 questions)
  2. Deploy via Typeform or self-built form
  3. AI chatbot conducts 1-on-1 in-depth interviews
  4. Target sample: 50-200 respondents

Key questions:
  - How do you currently solve this problem? (validate pain)
  - How much would you pay monthly? (validate willingness to pay)
  - Are you satisfied with current solutions? (validate differentiation)

Pass criteria:
  ✅ Response rate > 20%
  ✅ Willingness to pay > estimated delivery cost × 3
  ✅ > 40% of respondents dissatisfied with current solutions
```

### 4. 🔎 Competitor Analysis

```
Dimensions:
  - Competitor count and scale (Crunchbase data)
  - User review analysis (App Store / G2 / Trustpilot)
  - Pricing strategy analysis
  - Feature comparison matrix
  - Differentiation opportunity identification

Output:
  - Competitor list (with revenue estimates, user count, ratings)
  - Feature gap heatmap
  - Differentiation strategy recommendations
```

### 5. 🏗️ MVP Test

```
Flow:
  1. AI builds minimal viable prototype in 48 hours
  2. Deploy to 50-100 seed users
  3. Collect usage data and feedback

Metrics:
  - DAU/WAU (Daily/Weekly Active Users)
  - Core feature usage rate
  - NPS (Net Promoter Score)
  - Paid conversion rate

Pass criteria:
  ✅ WAU / registered users > 30%
  ✅ Core feature usage rate > 50%
  ✅ NPS > 20
```

---

## Decision Matrix

```
┌──────────┬───────────────────────────────────────────────┐
│  ✅ GO   │ Willingness to pay > delivery cost × 3         │
│ PROCEED  │ AND MVP conversion rate > 2%                   │
│          │ AND competitors < 10 OR differentiation > 0.7  │
├──────────┼───────────────────────────────────────────────┤
│  🔄 PIVOT│ Demand exists BUT:                             │
│          │ - Wrong target segment                          │
│          │ - Pricing model needs adjustment                │
│          │ - Technical feasibility uncertain               │
│          │ → Modify parameters and re-validate             │
├──────────┼───────────────────────────────────────────────┤
│  📦 STOP │ Insufficient demand signals                    │
│ ARCHIVE  │ OR market too small / declining                │
│          │ OR regulatory barriers too high                │
│          │ → Close, write lessons to Memory                │
└──────────┴───────────────────────────────────────────────┘
```

---

## Error Handling

| Error Code | Description | Recovery Strategy |
|------------|-------------|-------------------|
| `BUDGET_EXCEEDED` | Budget consumption reached cap | Stop all paid methods, return available results |
| `NO_TRAFFIC` | Landing page / ads get no traffic | Extend observation 3 days; still none → `inconclusive` |
| `SURVEY_LOW_RESPONSE` | Survey response rate < 5% | Switch survey channel or lower sample requirement |
| `MVP_BUILD_FAILURE` | MVP build failed | Downgrade to landing-page-only validation, log failure |

---

## Monitoring Metrics

```
demand_validate_budget_utilization_ratio        # Budget utilization ratio
demand_validate_methods_completed_total         # Validation methods completed count
demand_validate_proceed_rate                    # Proceed decision rate
demand_validate_archive_rate                    # Archive decision rate
demand_validate_duration_seconds                # Validation duration
```
