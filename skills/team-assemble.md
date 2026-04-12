# Skill: team-assemble

> Recruit, interview, and onboard human/AI team members
> 招募、面试并入职人类/AI 团队成员

## Definition

```yaml
skill:
  name: team-assemble
  version: 0.1.0
  description: "Build a team of humans and AI agents to execute on the validated opportunity"
  trigger:
    - event: "company_formed"
    - event: "staffing_need_identified"
    - manual: true
  inputs:
    - company_id: string
    - role_requirements: RoleRequirement[]
    - budget_per_role: float[]
    - team_composition: enum[ai_heavy, balanced, human_heavy]
  outputs:
    - team_roster: TeamMember[]
    - onboarding_status: OnboardingStatus[]
    - collaboration_workflow: Workflow
  dependencies:
    - company-form
```

## AI Worker Types / AI 工作者类型

| Type / 类型 | Role / 角色 | Capabilities / 能力 |
|-------------|------------|---------------------|
| Coder / 编码员 | Write and review code | Full-stack development, debugging, code review |
| Designer / 设计师 | Create UI/UX assets | Interface design, branding, illustration |
| Marketer / 营销员 | Content and campaigns | Copywriting, SEO, ad creation, social media |
| Support / 客服 | Customer inquiries | Chat support, email, FAQ management |
| Analyst / 分析师 | Data processing | Market research, financial analysis, reporting |
| PM / 产品经理 | Product coordination | Requirements, roadmap, stakeholder management |

## Human Recruitment Pipeline / 人类招聘流程

```
Role Definition → Job Posting → AI Screening → Interview → Offer → Onboarding
角色定义 → 职位发布 → AI 筛选 → 面试 → Offer → 入职
```

### Platforms / 平台
- Upwork, Toptal, Fiverr (freelancers / 自由职业者)
- LinkedIn, Indeed (full-time / 全职)
- GitHub, Dribbble (portfolio-based / 作品集)
- Dedicated Slack/Discord communities

## Human-AI Collaboration / 人类-AI 协作

### Orchestration Model / 编排模型
```
┌─────────────┐
│ AI Founder  │ ← Strategy & coordination
│ AI 创始人    │
├──────┬──────┤
│      │      │
│  AI Workers   │ ← High-volume, automated tasks
│  AI 工作者    │
│      │      │
├──────┴──────┤
│  Human Team  │ ← Creative, judgment, relationship tasks
│  人类团队     │
└─────────────┘
```

### Decision Rights / 决策权
- **AI-only:** Code implementation, data analysis, routine customer support
- **Human-required:** Final design approval, contract signing, customer escalations
- **Collaborative:** Product roadmap, pricing strategy, marketing direction
