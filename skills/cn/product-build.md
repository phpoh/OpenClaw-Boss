# 🦞 Skill: product-build（产品构建）

> **🇨🇳 中文版** | [🇬🇧 English](../en/product-build.md)

---

## 元信息

| 字段 | 值 |
|------|-----|
| 名称 | `product-build` |
| 版本 | `1.0.0` |
| 负责组件 | ⚡ 执行器（Executor） |
| 超时限制 | 1209600 秒（14 天） |
| 最大重试次数 | 2 |
| 上游依赖 | `team-assemble` |

---

## 概述

企业家"把想法变成产品"的能力。需求验证了、团队到位了，现在是交付时刻。OpenClaw 协调 AI 编码员和人类开发者，从规格到上线，全程自动化管理。

---

## 严格定义

```yaml
skill:
  name: product-build
  version: 1.0.0
  description: "设计、开发和交付解决已验证市场需求的产品"
  trigger:
    - event: "team_assembled"
    - event: "sprint_start"
    - event: "iteration_needed"
    - manual: true
  inputs:
    - name: company_id
      type: "string"
      required: true
    - name: validated_requirements
      type: "list[Requirement]"
      required: true
      description: "已验证的产品需求列表"
    - name: team_roster
      type: "list[TeamMember]"
      required: true
      description: "团队成员列表"
    - name: sprint_duration_days
      type: "int"
      required: false
      default: 7
      constraints: "1 <= value <= 14"
    - name: mvp_scope
      type: "enum[minimal, standard, full]"
      required: false
      default: "minimal"
      description: "MVP 范围：最小（核心功能）、标准（核心+辅助）、完整（全功能）"
  outputs:
    - name: product_artifacts
      type: "list[Artifact]"
      description: "产品制品列表（代码、文档、配置）"
    - name: deployment_url
      type: "string"
      description: "产品访问地址"
    - name: test_results
      type: "TestReport"
      description: "测试报告"
    - name: user_metrics_baseline
      type: "MetricsBaseline"
      description: "上线后初始指标基线"
  dependencies:
    skills: ["team-assemble"]
    tools:
      - code_editor          # AI 代码编辑器
      - test_runner          # 自动化测试
      - ci_cd_pipeline       # CI/CD 流水线
      - deployment_manager   # 部署管理
      - monitoring_setup     # 监控配置
```

---

## 开发生命周期

```
📋 规格 → 🏗️ 设计 → 💻 构建 → 🧪 测试 → 🚀 部署 → 📈 衡量 → 💡 学习 → [迭代]
```

### 阶段一：规格定义 📋

```
输入：validated_requirements（来自 demand-validate）
输出：ProductSpec（产品规格文档）

自动执行：
  [ ] 将用户需求转化为用户故事（User Stories）
  [ ] 定义验收标准（Acceptance Criteria）
  [ ] 拆解为可执行的任务（Tasks），每个 <= 4 小时
  [ ] 估算工时并排定优先级（MoSCoW 方法）
  [ ] 生成技术规格文档

  🛑 人类审批：确认产品范围和优先级
```

### 阶段二：架构设计 🏗️

```
输出：ArchitectureDoc（架构文档）

自动执行：
  [ ] 选择技术栈（根据项目特征自动推荐）
  [ ] 设计系统架构图
  [ ] 定义 API 契约（OpenAPI Spec）
  [ ] 设计数据库 Schema
  [ ] 定义安全策略（认证、授权、数据加密）
  [ ] 生成基础设施即代码（IaC）模板
```

**技术栈推荐矩阵：**

| 项目类型 | 前端 | 后端 | 数据库 | 部署 |
|---------|------|------|--------|------|
| Web SaaS | Next.js + Tailwind | FastAPI / Node.js | PostgreSQL + Redis | Vercel + AWS |
| 移动优先 | React Native | Go / Python | PostgreSQL | AWS + CloudFront |
| API 服务 | - | FastAPI / Go | PostgreSQL + Redis | AWS Lambda |
| 数据平台 | React + D3.js | Python | ClickHouse + PG | AWS + dbt |

### 阶段三：编码构建 💻

```
分配策略：
  - AI 编码员：实现标准 CRUD、API 端点、单元测试、文档
  - 人类开发者：复杂业务逻辑、算法优化、安全审计
  - 协作完成：UI 实现、集成测试、性能调优

代码质量保障：
  [ ] 提交前自动 Lint + Format（ESLint, Prettier, Black）
  [ ] AI Code Review（每次 PR 自动审查）
  [ ] 安全扫描（依赖漏洞检测）
  [ ] 代码覆盖率目标：> 80%
```

### 阶段四：测试验证 🧪

```
测试金字塔：
          ╱  E2E 测试  ╲          ← 少量，关键路径
         ╱   集成测试    ╲         ← 适量，API 和模块间
        ╱    单元测试      ╲        ← 大量，核心逻辑
       ╱___________________╲

测试标准：
  [ ] 单元测试覆盖率 > 80%
  [ ] API 集成测试通过率 100%
  [ ] E2E 测试覆盖核心用户路径
  [ ] 性能测试：P95 延迟 < 500ms
  [ ] 安全测试：无高危漏洞
  [ ] 兼容性测试：主流浏览器/设备
```

### 阶段五：部署上线 🚀

```
部署策略：
  1. Canary（金丝雀发布）→ 5% 流量
  2. 观察 30 分钟，检查错误率
  3. Blue-Green（蓝绿部署）→ 50% 流量
  4. 观察 2 小时
  5. 全量发布

部署检查清单：
  [ ] CI 流水线全部通过
  [ ] 数据库迁移已执行且可回滚
  [ ] 环境变量已配置
  [ ] 监控和告警已设置
  [ ] 回滚方案已测试
```

### 阶段六：衡量学习 📈

```
上线后 7 天内自动收集：
  - 用户注册量和激活率
  - 核心功能使用频率
  - 错误率和崩溃日志
  - 用户反馈（NPS 调研）

自动生成：
  - 周度产品报告
  - 功能使用热力图
  - 迭代优先级建议
```

---

## 错误处理

| 错误码 | 描述 | 处理策略 |
|--------|------|---------|
| `BUILD_FAILED` | 构建失败 | AI 分析错误日志，自动修复；3 次失败后通知人类 |
| `TEST_FAILED` | 测试未通过 | 阻止部署，自动分配给对应开发者修复 |
| `DEPLOY_FAILED` | 部署失败 | 自动回滚到上一稳定版本，分析原因 |
| `PERFORMANCE_DEGRADED` | 性能低于标准 | 自动扩容或优化代码，告警通知 |
| `SECURITY_VULNERABILITY` | 发现安全漏洞 | 阻止部署，标记为紧急修复任务 |

---

## 监控指标

```
product_build_cycle_time_seconds                # 开发周期耗时
product_build_deploy_frequency                  # 部署频率
product_build_test_pass_rate                    # 测试通过率
product_build_bug_escape_rate                   # Bug 逃逸率
product_build_deployment_success_rate           # 部署成功率
product_build_mean_time_to_recovery_seconds     # 平均恢复时间
```
