# 🦞 Skill: company-form（公司组建）

> **🇨🇳 中文版** | [🇬🇧 English](../en/company-form.md)

---

## 元信息

| 字段 | 值 |
|------|-----|
| 名称 | `company-form` |
| 版本 | `1.0.0` |
| 负责组件 | ⚡ 执行器（Executor） |
| 超时限制 | 86400 秒（24 小时，含审批等待） |
| 最大重试次数 | 1 |
| 上游依赖 | `demand-validate`（decision=proceed） |

---

## 概述

企业家"跑腿注册"的能力。需求验证通过后，OpenClaw 自动搭建公司基础设施——注册实体、开户、合规、品牌——让公司从 0 到 1 跑起来。

---

## 严格定义

```yaml
skill:
  name: company-form
  version: 1.0.0
  description: "为新企业建立法律、财务和运营基础设施"
  trigger:
    - event: "demand_validated"              # demand-validate 决策为 proceed
    - event: "validation_decision_proceed"
    - manual: true
  inputs:
    - name: opportunity_id
      type: "string"
      required: true
      description: "已通过验证的机会 ID"
    - name: venture_name
      type: "string"
      required: true
      constraints: "2 <= length <= 50"
      description: "新公司名称"
    - name: entity_type
      type: "enum[LLC, C-Corp, Sole_Proprietor, LTD, 个体工商户, 有限责任公司]"
      required: true
      description: "法律实体类型"
    - name: jurisdiction
      type: "string"
      required: true
      description: "注册管辖区（如 US-DE, CN-SH, SG）"
    - name: initial_budget
      type: "float"
      required: true
      constraints: "value >= 100"
      description: "初始预算（美元）"
    - name: brand_preferences
      type: "object"
      required: false
      description: "品牌偏好（风格、颜色、行业属性）"
  outputs:
    - name: company_id
      type: "string"
      description: "公司唯一标识符"
    - name: legal_status
      type: "enum[pending, registered, active, failed]"
      description: "法律注册状态"
    - name: infrastructure_status
      type: "InfrastructureChecklist"
      description: "各项基础设施搭建状态"
    - name: total_setup_cost
      type: "float"
      description: "搭建总成本"
  dependencies:
    skills: ["demand-validate"]
    tools:
      - stripe_atlas_api         # Stripe Atlas 注册
      - firstbase_api            # Firstbase.io 注册
      - domain_registrar_api     # 域名注册
      - design_generator         # AI 品牌设计生成
      - legal_doc_generator      # 法律文档生成
```

---

## 前置条件

```
PRE 1: opportunity_id 对应的验证决策为 proceed
PRE 2: venture_name 未被其他公司使用（域名 + 商标检查）
PRE 3: 人类已审批公司注册（此技能包含多个人工审批点）
PRE 4: initial_budget >= 最低注册成本（管辖区相关）
```

## 后置条件

```
POST 1: 公司法律实体已注册或状态为 pending（等待审批中）
POST 2: 域名已注册且 DNS 已配置
POST 3: 品牌资产已生成（Logo、配色方案）
POST 4: 总花费记录到财务系统
POST 5: 公司信息写入记忆库
```

---

## 执行清单

### 阶段一：合规预检 🛡️

```
[ ] 检查 venture_name 商标冲突（USPTO / 中国商标网）
[ ] 检查域名可用性（.com / .io / .ai / .cn）
[ ] 检查目标管辖区注册要求
[ ] 确认 initial_budget 覆盖最低注册成本
[ ] 生成合规检查报告 → 🛑 提交人类审批
```

### 阶段二：法律注册 ⚖️

```
根据管辖区自动选择注册路径：

  US (Delaware):
    → Stripe Atlas / Firstbase 自动化注册
    → 费用：$500 + 州费

  CN (上海/深圳):
    → 生成注册材料清单
    → 🛑 人类线下提交或使用代办服务

  SG (Singapore):
    → Stripe Atlas / 本地服务
    → 费用：$300-$800

注册包含：
  [ ] 法律实体注册
  [ ] EIN / 统一社会信用代码申请
  [ ] 注册代理人设置
  [ ] 公司章程 / 营业执照
```

### 阶段三：金融基础设施 🏦

```
[ ] 开设企业银行账户
    → 🛑 需要人类提供身份验证材料
[ ] 配置 Stripe / 支付宝 / 微信支付 收款
[ ] 设置记账软件（QuickBooks / Xero / 飞书财务）
[ ] 建立支出分类体系
[ ] 设置预算告警阈值
```

### 阶段四：品牌与数字资产 🎨

```
AI 自动执行（无需人类审批）：
  [ ] AI 生成 Logo 方案（3 个备选）
  [ ] 确定品牌配色方案
  [ ] 生成品牌字体搭配
  [ ] 创建品牌指南文档（Brand Guidelines）

  🛑 人类审批：选择最终 Logo 方案

自动执行：
  [ ] 注册域名并配置 DNS
  [ ] 配置企业邮箱（Google Workspace / 飞书）
  [ ] 搭建公司官网 / 着陆页
  [ ] 创建社交媒体账号（Twitter / LinkedIn / 公众号）
```

### 阶段五：运营工具 ⚙️

```
[ ] 项目管理工具（Linear / Notion / 飞书）
[ ] 代码仓库（GitHub / GitLab）
[ ] 通讯工具（Slack / Discord / 飞书）
[ ] 监控告警（Sentry / Datadog）
[ ] 文档协作（Notion / Confluence）
```

---

## 人类审批点汇总

```
🛑 必须人类审批：
  1. 合规预检报告确认
  2. 法律实体注册确认
  3. 银行账户开设（需提供身份材料）
  4. 最终 Logo/品牌方案选择

✅ AI 可自动执行：
  1. 商标和域名检查
  2. 品牌设计方案生成
  3. 数字资产注册（域名、邮箱、社交账号）
  4. 运营工具配置
  5. 法律文档模板生成
```

---

## 错误处理

| 错误码 | 描述 | 处理策略 |
|--------|------|---------|
| `NAME_CONFLICT` | 公司名称与现有商标冲突 | 生成 3 个替代名称，暂停等待人类选择 |
| `REGISTRATION_FAILED` | 法律注册被拒绝 | 记录原因，建议替代管辖区或实体类型 |
| `BANK_REJECTED` | 银行开户被拒 | 列出替代银行，等待人类重新申请 |
| `DOMAIN_TAKEN` | 首选域名已被注册 | 自动尝试 .io / .ai / .co 等替代后缀 |
| `BUDGET_INSUFFICIENT` | 初始预算不足 | 列出最低启动成本，等待人类追加预算 |

---

## 监控指标

```
company_form_total_duration_seconds             # 总搭建耗时
company_form_steps_completed_ratio              # 完成步骤比例
company_form_human_approval_wait_seconds        # 等待人类审批耗时
company_form_total_cost_usd                     # 搭建总成本
```
