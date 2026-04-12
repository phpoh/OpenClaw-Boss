# Skill: product-build

> Design, develop, and deliver products or services
> 设计、开发和交付产品或服务

## Definition

```yaml
skill:
  name: product-build
  version: 0.1.0
  description: "Design, develop, and deliver the product that addresses validated market needs"
  trigger:
    - event: "team_assembled"
    - event: "sprint_start"
    - manual: true
  inputs:
    - company_id: string
    - validated_requirements: Requirement[]
    - technical_constraints: Constraint[]
    - timeline_days: int
  outputs:
    - product_artifacts: Artifact[]
    - deployment_status: DeploymentStatus
    - user_feedback: Feedback[]
  dependencies:
    - team-assemble
```

## Development Lifecycle / 开发生命周期

```
Spec → Design → Build → Test → Deploy → Measure → Learn → [Iterate]
规格 → 设计 → 构建 → 测试 → 部署 → 衡量 → 学习 → [迭代]
```

### 1. Specification / 规格定义
- Convert validated demand into product requirements
- Define user stories and acceptance criteria
- Create technical specification document
- Estimate effort and timeline

### 2. Design / 设计
- System architecture design
- Database schema design
- API contract definition
- UI/UX wireframes and prototypes

### 3. Build / 构建
- AI coding agents implement features
- Human developers handle complex logic
- Automated code review via AI
- Continuous integration setup

### 4. Test / 测试
- Automated unit and integration tests
- AI-powered regression testing
- User acceptance testing (small group)
- Performance and security testing

### 5. Deploy / 部署
- Infrastructure as Code (Terraform, Pulumi)
- CI/CD pipeline configuration
- Monitoring and alerting setup
- Gradual rollout (canary → blue-green → full)

### 6. Measure & Learn / 衡量与学习
- User behavior analytics
- A/B testing framework
- Feedback collection and categorization
- Iteration planning based on data

## Tech Stack Preferences / 技术栈偏好

| Layer | Preferred Options |
|-------|-------------------|
| Frontend | Next.js, React, Tailwind CSS |
| Backend | Python (FastAPI), Node.js, Go |
| Database | PostgreSQL, Redis, S3 |
| Infrastructure | Vercel, AWS, Cloudflare |
| Monitoring | Datadog, Sentry, PostHog |
