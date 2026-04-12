# 🦞 Skill: company-form

> **🇬🇧 English** | [🇨🇳 中文版](../cn/company-form.md)

---

## Metadata

| Field | Value |
|-------|-------|
| Name | `company-form` |
| Version | `1.0.0` |
| Owner | ⚡ Executor |
| Timeout | 86400s (24 hours, includes approval waits) |
| Max Retries | 1 |
| Upstream Dependency | `demand-validate` (decision=proceed) |

---

## Overview

The entrepreneur's "legwork." After demand is validated, OpenClaw automatically builds the company infrastructure — entity registration, banking, compliance, branding — taking the venture from 0 to 1.

---

## Strict Definition

```yaml
skill:
  name: company-form
  version: 1.0.0
  description: "Establish legal, financial, and operational infrastructure for a new venture"
  trigger:
    - event: "demand_validated"
    - event: "validation_decision_proceed"
    - manual: true
  inputs:
    - name: opportunity_id
      type: "string"
      required: true
      description: "Validated opportunity ID"
    - name: venture_name
      type: "string"
      required: true
      constraints: "2 <= length <= 50"
      description: "New company name"
    - name: entity_type
      type: "enum[LLC, C-Corp, Sole_Proprietor, LTD]"
      required: true
      description: "Legal entity type"
    - name: jurisdiction
      type: "string"
      required: true
      description: "Registration jurisdiction (e.g. US-DE, CN-SH, SG)"
    - name: initial_budget
      type: "float"
      required: true
      constraints: "value >= 100"
      description: "Initial budget (USD)"
    - name: brand_preferences
      type: "object"
      required: false
      description: "Brand preferences (style, colors, industry attributes)"
  outputs:
    - name: company_id
      type: "string"
      description: "Unique company identifier"
    - name: legal_status
      type: "enum[pending, registered, active, failed]"
      description: "Legal registration status"
    - name: infrastructure_status
      type: "InfrastructureChecklist"
      description: "Status of each infrastructure component"
    - name: total_setup_cost
      type: "float"
      description: "Total setup cost"
  dependencies:
    skills: ["demand-validate"]
    tools:
      - stripe_atlas_api
      - firstbase_api
      - domain_registrar_api
      - design_generator
      - legal_doc_generator
```

---

## Pre-conditions

```
PRE 1: opportunity_id has validation decision = proceed
PRE 2: venture_name not in use (domain + trademark check)
PRE 3: Human has approved company formation (multiple approval points)
PRE 4: initial_budget >= minimum registration cost for jurisdiction
```

## Post-conditions

```
POST 1: Legal entity registered or status pending
POST 2: Domain registered and DNS configured
POST 3: Brand assets generated (Logo, color palette)
POST 4: Total cost recorded in financial system
POST 5: Company info written to Memory
```

---

## Execution Checklist

### Phase 1: Compliance Pre-check 🛡️

```
[ ] Check trademark conflicts (USPTO / local trademark DB)
[ ] Check domain availability (.com / .io / .ai / .cn)
[ ] Verify registration requirements for target jurisdiction
[ ] Confirm initial_budget covers minimum registration cost
[ ] Generate compliance report → 🛑 Submit for human approval
```

### Phase 2: Legal Registration ⚖️

```
Auto-select registration path based on jurisdiction:

  US (Delaware):
    → Stripe Atlas / Firstbase automated registration
    → Cost: $500 + state fees

  SG (Singapore):
    → Stripe Atlas / local service
    → Cost: $300-$800

  Other:
    → Generate registration document checklist
    → 🛑 Human submits offline or via agency

Includes:
  [ ] Legal entity registration
  [ ] EIN / tax ID application
  [ ] Registered agent setup
  [ ] Articles of incorporation / operating agreement
```

### Phase 3: Financial Infrastructure 🏦

```
[ ] Open business bank account
    → 🛑 Requires human identity verification documents
[ ] Configure Stripe / PayPal payment processing
[ ] Set up accounting software (QuickBooks / Xero)
[ ] Establish expense categorization system
[ ] Set budget alert thresholds
```

### Phase 4: Brand & Digital Assets 🎨

```
AI auto-executes (no human approval needed):
  [ ] AI generates Logo options (3 alternatives)
  [ ] Define brand color palette
  [ ] Generate brand typography pairings
  [ ] Create Brand Guidelines document

  🛡️ Human approval: Select final Logo option

Auto-executes:
  [ ] Register domain and configure DNS
  [ ] Configure business email (Google Workspace)
  [ ] Build company website / landing page
  [ ] Create social media accounts (Twitter / LinkedIn)
```

### Phase 5: Operations Tooling ⚙️

```
[ ] Project management (Linear / Notion)
[ ] Code repository (GitHub / GitLab)
[ ] Communication (Slack / Discord)
[ ] Monitoring & alerting (Sentry / Datadog)
[ ] Documentation (Notion / Confluence)
```

---

## Human Approval Points

```
🛡️ Requires human approval:
  1. Compliance pre-check report confirmation
  2. Legal entity registration confirmation
  3. Bank account opening (identity documents needed)
  4. Final Logo/brand selection

✅ AI can auto-execute:
  1. Trademark and domain checks
  2. Brand design generation
  3. Digital asset registration (domain, email, social)
  4. Operations tooling setup
  5. Legal document template generation
```

---

## Error Handling

| Error Code | Description | Recovery Strategy |
|------------|-------------|-------------------|
| `NAME_CONFLICT` | Name conflicts with existing trademark | Generate 3 alternative names, pause for human choice |
| `REGISTRATION_FAILED` | Legal registration rejected | Log reason, suggest alternative jurisdiction or entity type |
| `BANK_REJECTED` | Bank account application rejected | List alternative banks, wait for human re-application |
| `DOMAIN_TAKEN` | Primary domain already registered | Auto-try alternatives (.io / .ai / .co) |
| `BUDGET_INSUFFICIENT` | Initial budget too low | Show minimum startup cost, wait for human to add budget |

---

## Monitoring Metrics

```
company_form_total_duration_seconds             # Total setup duration
company_form_steps_completed_ratio              # Steps completion ratio
company_form_human_approval_wait_seconds        # Time waiting for human approval
company_form_total_cost_usd                     # Total setup cost
```
