# 🦞 Skill: team-assemble

> **🇬🇧 English** | [🇨🇳 中文版](../cn/team-assemble.md)

---

## Metadata

| Field | Value |
|-------|-------|
| Name | `team-assemble` |
| Version | `1.0.0` |
| Owner | ⚡ Executor |
| Timeout | 604800s (7 days, includes hiring waits) |
| Max Retries | 1 |
| Upstream Dependency | `company-form` |

---

## Overview

The entrepreneur's core skill of "getting people to do the work." After deciding what to build, the key is **finding the right people** — human and AI collaborators. OpenClaw acts like a real boss: define roles, post jobs, screen candidates, assign tasks.

---

## Strict Definition

```yaml
skill:
  name: team-assemble
  version: 1.0.0
  description: "Build a team of humans and AI agents to execute on the validated opportunity"
  trigger:
    - event: "company_formed"
    - event: "infrastructure_ready"
    - event: "staffing_need_identified"
    - manual: true
  inputs:
    - name: company_id
      type: "string"
      required: true
      description: "Company unique identifier"
    - name: product_requirements
      type: "list[Requirement]"
      required: true
      description: "Product requirements list (inherited from demand-validate)"
    - name: team_composition
      type: "enum[ai_heavy, balanced, human_heavy]"
      required: false
      default: "balanced"
      description: "Team composition preference"
    - name: monthly_budget
      type: "float"
      required: true
      constraints: "value >= 500"
      description: "Monthly team budget (USD)"
    - name: timeline_weeks
      type: "int"
      required: false
      default: 8
      constraints: "1 <= value <= 52"
      description: "Expected delivery timeline (weeks)"
  outputs:
    - name: team_roster
      type: "list[TeamMember]"
      description: "Team member roster"
    - name: task_assignments
      type: "list[TaskAssignment]"
      description: "Task assignment plan"
    - name: collaboration_workflow
      type: "Workflow"
      description: "Collaboration workflow definition"
    - name: monthly_cost_estimate
      type: "float"
      description: "Estimated monthly personnel cost"
  dependencies:
    skills: ["company-form"]
    tools:
      - upwork_api
      - toptal_api
      - github_api
      - ai_agent_manager
      - communication_setup
```

---

## AI Worker Agent Matrix

```
┌──────────┬──────────┬──────────────────────┬─────────────┐
│ Type     │ Level    │ Use Case             │ Est. Monthly │
├──────────┼──────────┼──────────────────────┼─────────────┤
│ 💻 Coder │ L1: Base │ CRUD, simple pages   │ $50-200     │
│          │ L2: Adv  │ Complex logic, arch   │ $200-800    │
│          │ L3: Exp  │ System design, perf   │ $800-2000   │
├──────────┼──────────┼──────────────────────┼─────────────┤
│ 🎨 Design│ L1: Base │ Icons, simple layouts │ $30-100     │
│          │ L2: Adv  │ Full UI/UX design     │ $100-400    │
│          │ L3: Exp  │ Brand systems, motion │ $400-1000   │
├──────────┼──────────┼──────────────────────┼─────────────┤
│ 📢 Market│ L1: Base │ Social media posts    │ $30-100     │
│          │ L2: Adv  │ SEO, ad strategy      │ $100-500    │
│          │ L3: Exp  │ Brand strategy, growth│ $500-1500   │
├──────────┼──────────┼──────────────────────┼─────────────┤
│ 🎧 Supp  │ L1: Base │ FAQ auto-reply        │ $20-50      │
│          │ L2: Adv  │ Complex issue handling│ $50-200     │
├──────────┼──────────┼──────────────────────┼─────────────┤
│ 📊 Analy │ L1: Base │ Data cleanup, reports │ $30-100     │
│          │ L2: Adv  │ Market research, model│ $100-500    │
└──────────┴──────────┴──────────────────────┴─────────────┘
```

---

## Human Recruitment Pipeline

```
┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
│ 📋 Define │ →  │ 📮 Post   │ →  │ 🤖 Screen │ →  │ 👔 Interview│ → │ 🎓 Onboard│
│ 1-2 hrs   │    │ Auto     │    │ Auto     │    │ 1-3 days  │    │ 1 day    │
└──────────┘    └──────────┘    └──────────┘    └──────────┘    └──────────┘
```

### Stage Details

**📋 Role Definition:**
- Auto-decompose product requirements into role list
- Each role: skill requirements, experience level, budget range, deliverables

**📮 Job Posting:**
- Auto-generate JD (Job Description)
- Post to Upwork / Toptal / LinkedIn / Fiverr
- Set auto-close timer (7 days)

**🤖 AI Screening:**
- Score candidates by portfolio, ratings, completion rate
- Filter out candidates with rating < 4.5 or completion < 90%
- Auto-send screening questions to top 5

**👔 Interview:**
- AI pre-interview: assess technical skills via chat/video
- 🛡️ Human final interview: final candidate requires human confirmation
- Auto-generate interview scorecard

**🎓 Onboarding:**
- Auto-send project context document
- Configure tool access (GitHub / Slack / Notion)
- Assign initial tasks with deadlines

---

## Task Assignment Algorithm

```python
def assign_tasks(team, tasks):
    """Assign tasks based on skill match and cost efficiency"""
    assignments = []
    for task in sorted(tasks, key=lambda t: t.priority, reverse=True):
        best_match = None
        best_score = 0
        for member in team.available_members():
            score = (
                skill_match(member.skills, task.required_skills) * 0.40 +
                cost_efficiency(member.rate, task.complexity) * 0.25 +
                availability(member.current_load) * 0.20 +
                past_performance(member.id, task.domain) * 0.15
            )
            if score > best_score:
                best_score = score
                best_match = member
        if best_match:
            assignments.append(Assignment(task, best_match, best_score))
    return assignments
```

---

## Error Handling

| Error Code | Description | Recovery Strategy |
|------------|-------------|-------------------|
| `NO_CANDIDATES` | No qualified candidates | Expand search, increase budget, extend hiring period |
| `CANDIDATE_REJECTED` | Candidate declined offer | Auto-contact backup candidates |
| `MEMBER_UNRESPONSIVE` | Member unresponsive (>48h) | Flag alert, start replacement hiring |
| `BUDGET_EXCEEDED` | Personnel cost exceeds monthly budget | Downgrade AI agent levels or reduce human headcount |
| `SKILL_MISMATCH` | Skill mismatch discovered post-assignment | Re-evaluate and reassign tasks |

---

## Monitoring Metrics

```
team_assemble_hiring_duration_seconds           # Hiring duration
team_assemble_fill_rate                         # Position fill rate
team_assemble_cost_per_role_usd                 # Cost per role monthly
team_assemble_task_completion_rate               # Task completion rate
team_assemble_human_satisfaction_score           # Human member satisfaction
```
