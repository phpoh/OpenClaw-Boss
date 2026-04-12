# 🦞 Skill: team-assemble（团队组建）

> **🇨🇳 中文版** | [🇬🇧 English](../en/team-assemble.md)

---

## 元信息

| 字段 | 值 |
|------|-----|
| 名称 | `team-assemble` |
| 版本 | `1.0.0` |
| 负责组件 | ⚡ 执行器（Executor） |
| 超时限制 | 604800 秒（7 天，含招聘等待） |
| 最大重试次数 | 1 |
| 上游依赖 | `company-form` |

---

## 概述

企业家"找人干活"的核心能力。确定要做什么之后，关键就是**找到对的人**——人类和 AI 协作者。OpenClaw 像真正的老板一样：定义角色、发布招聘、筛选面试、分配任务。

---

## 严格定义

```yaml
skill:
  name: team-assemble
  version: 1.0.0
  description: "组建由人类和 AI 代理组成的团队来执行已验证的机会"
  trigger:
    - event: "company_formed"
    - event: "infrastructure_ready"
    - event: "staffing_need_identified"
    - manual: true
  inputs:
    - name: company_id
      type: "string"
      required: true
      description: "公司唯一标识符"
    - name: product_requirements
      type: "list[Requirement]"
      required: true
      description: "产品需求列表（从 demand-validate 继承）"
    - name: team_composition
      type: "enum[ai_heavy, balanced, human_heavy]"
      required: false
      default: "balanced"
      description: "团队构成偏好"
    - name: monthly_budget
      type: "float"
      required: true
      constraints: "value >= 500"
      description: "团队月度预算（美元）"
    - name: timeline_weeks
      type: "int"
      required: false
      default: 8
      constraints: "1 <= value <= 52"
      description: "预期交付周期（周）"
  outputs:
    - name: team_roster
      type: "list[TeamMember]"
      description: "团队成员花名册"
    - name: task_assignments
      type: "list[TaskAssignment]"
      description: "任务分配方案"
    - name: collaboration_workflow
      type: "Workflow"
      description: "协作工作流定义"
    - name: monthly_cost_estimate
      type: "float"
      description: "预估月度人力成本"
  dependencies:
    skills: ["company-form"]
    tools:
      - upwork_api              # Upwork 自由职业者平台
      - toptal_api              # Toptal 高端人才平台
      - github_api              # GitHub 开发者搜索
      - ai_agent_manager        # AI 代理管理器
      - communication_setup     # 通讯工具配置
```

---

## AI 工作者代理矩阵

```
┌──────────┬──────────┬──────────────────────┬─────────────┐
│ 类型      │ 能力等级  │ 适用场景              │ 月成本估算    │
├──────────┼──────────┼──────────────────────┼─────────────┤
│ 💻 编码员  │ L1: 基础  │ CRUD、简单页面        │ $50-200     │
│          │ L2: 高级  │ 复杂业务逻辑、架构     │ $200-800    │
│          │ L3: 专家  │ 系统设计、性能优化     │ $800-2000   │
├──────────┼──────────┼──────────────────────┼─────────────┤
│ 🎨 设计师 │ L1: 基础  │ 图标、简单排版        │ $30-100     │
│          │ L2: 高级  │ 完整 UI/UX 设计       │ $100-400    │
│          │ L3: 专家  │ 品牌系统、动效设计     │ $400-1000   │
├──────────┼──────────┼──────────────────────┼─────────────┤
│ 📢 营销员 │ L1: 基础  │ 社交媒体帖子         │ $30-100     │
│          │ L2: 高级  │ SEO、广告投放策略     │ $100-500    │
│          │ L3: 专家  │ 品牌策略、增长黑客     │ $500-1500   │
├──────────┼──────────┼──────────────────────┼─────────────┤
│ 🎧 客服   │ L1: 基础  │ FAQ 自动回复         │ $20-50      │
│          │ L2: 高级  │ 复杂问题处理         │ $50-200     │
├──────────┼──────────┼──────────────────────┼─────────────┤
│ 📊 分析师 │ L1: 基础  │ 数据整理、报表生成    │ $30-100     │
│          │ L2: 高级  │ 市场研究、财务建模    │ $100-500    │
└──────────┴──────────┴──────────────────────┴─────────────┘
```

---

## 人类招聘流程

```
┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
│ 📋 角色定义│ →  │ 📮 职位发布│ →  │ 🤖 AI 筛选│ →  │ 👔 面试   │ →  │ 🎓 入职   │
│ 1-2 小时  │    │ 自动     │    │ 自动     │    │ 1-3 天   │    │ 1 天     │
└──────────┘    └──────────┘    └──────────┘    └──────────┘    └──────────┘
```

### 各阶段详细规则

**📋 角色定义：**
- 根据产品需求自动拆解为角色列表
- 每个角色包含：技能要求、经验等级、预算范围、交付物定义

**📮 职位发布：**
- 自动生成 JD（职位描述）
- 同步发布到 Upwork / Toptal / LinkedIn / Fiverr
- 设置自动关闭时间（7 天）

**🤖 AI 筛选：**
- 根据作品集、评分、历史完成率自动打分
- 排除评分 < 4.5 / 完成率 < 90% 的候选人
- 自动向 Top 5 候选人发送筛选问题

**👔 面试：**
- AI 预面试：通过聊天/视频评估技术能力
- 🛑 人类终面：最终人选需人类确认
- 面试评分表自动生成

**🎓 入职：**
- 自动发送项目背景文档
- 配置工具访问权限（GitHub / Slack / Notion）
- 分配初始任务并设定截止日期

---

## 任务分配算法

```python
def assign_tasks(team, tasks):
    """根据能力匹配度和成本效率分配任务"""
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

## 错误处理

| 错误码 | 描述 | 处理策略 |
|--------|------|---------|
| `NO_CANDIDATES` | 无合格候选人 | 扩大搜索范围，提高预算，延长招聘期 |
| `CANDIDATE_REJECTED` | 候选人拒绝 Offer | 自动联系备选候选人 |
| `MEMBER_UNRESPONSIVE` | 团队成员失联（>48h） | 标记告警，启动替代招聘 |
| `BUDGET_EXCEEDED` | 人力成本超出月度预算 | 降级 AI 代理等级或减少人类成员 |
| `SKILL_MISMATCH` | 分配后发现能力不匹配 | 重新评估并调整任务分配 |

---

## 监控指标

```
team_assemble_hiring_duration_seconds           # 招聘耗时
team_assemble_fill_rate                         # 岗位填充率
team_assemble_cost_per_role_usd                 # 每角色月成本
team_assemble_task_completion_rate               # 任务完成率
team_assemble_human_satisfaction_score           # 人类成员满意度
```
