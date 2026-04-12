# Skill: company-form

> Register company, set up legal and financial infrastructure
> 注册公司，搭建法律和财务基础设施

## Definition

```yaml
skill:
  name: company-form
  version: 0.1.0
  description: "Establish legal entity, banking, compliance, and operational infrastructure"
  trigger:
    - event: "demand_validated"
    - manual: true
  inputs:
    - venture_name: string
    - entity_type: enum[LLC, C-Corp, Sole_Proprietor, LTD]
    - jurisdiction: string
    - initial_budget: float
  outputs:
    - company_id: string
    - legal_docs: Document[]
    - banking_status: enum[pending, active]
    - compliance_checklist: Checklist
  dependencies:
    - demand-validate
```

## Setup Checklist / 搭建清单

### Legal / 法律
- [ ] Register legal entity (via Stripe Atlas, Firstbase, or local services)
- [ ] File articles of incorporation / organization
- [ ] Obtain EIN / tax registration number
- [ ] Draft operating agreement / bylaws
- [ ] Register for applicable business licenses
- [ ] Set up registered agent (if required)
- [ ] Create terms of service and privacy policy

### Financial / 财务
- [ ] Open business bank account
- [ ] Set up accounting software (QuickBooks, Xero, etc.)
- [ ] Configure invoicing and payment processing
- [ ] Establish bookkeeping practices
- [ ] Set up expense tracking and categorization

### Operations / 运营
- [ ] Register domain name and set up email
- [ ] Create brand identity (name, logo, color palette)
- [ ] Build company website / landing page
- [ ] Set up communication tools (email, chat, project management)
- [ ] Create standard operating procedures

## Risk Controls / 风险控制

```
HUMAN_APPROVAL_REQUIRED for:
  - Legal entity registration
  - Bank account opening
  - Contracts > $1,000
  - IP licensing agreements

AUTO_ALLOWED for:
  - Domain registration (< $50)
  - Software subscriptions (< $100/mo)
  - Basic brand asset creation
  - Standard template document generation
```
