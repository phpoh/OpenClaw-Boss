# 🦞 Skill: demand-validate（需求验证）

> **🇨🇳 中文版** | [🇬🇧 English](../en/demand-validate.md)

---

## 元信息

| 字段 | 值 |
|------|-----|
| 名称 | `demand-validate` |
| 版本 | `1.0.0` |
| 负责组件 | 🧭 规划器（Planner） |
| 执行者 | ⚡ 执行器（Executor） |
| 超时限制 | 7200 秒（2 小时） |
| 最大重试次数 | 2 |
| 上游依赖 | `market-sense` |

---

## 概述

企业家的"权衡"能力。发现机会后，不冲动投入——先验证：这个需求是真的吗？人们愿意付多少钱？竞品做得怎么样？用最小成本获取最大确定性。

---

## 严格定义

```yaml
skill:
  name: demand-validate
  version: 1.0.0
  description: "以最小成本验证市场机会是否代表真实、可变现的需求"
  trigger:
    - event: "opportunity_identified"        # market-sense 触发
    - event: "opportunity_score_above_0.6"   # 高分机会自动触发
    - manual: true
  inputs:
    - name: opportunity_id
      type: "string"
      required: true
      description: "market-sense 输出的机会 ID"
    - name: validation_methods
      type: "list[enum[landing_page, ad_test, survey, competitor_analysis, mvp]]"
      required: false
      default: ["landing_page", "competitor_analysis", "survey"]
      description: "要执行的验证方法组合"
    - name: budget_limit
      type: "float"
      required: true
      constraints: "value >= 0"
      description: "验证阶段预算上限（美元）"
    - name: duration_days
      type: "int"
      required: false
      default: 7
      constraints: "1 <= value <= 30"
      description: "验证周期（天）"
  outputs:
    - name: validation_report
      type: "ValidationReport"
      description: "完整的验证报告"
    - name: decision
      type: "enum[proceed, pivot, archive]"
      description: "Go/No-Go 决策"
    - name: refined_estimate
      type: "MarketEstimate"
      description: "修正后的市场估算"
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

## 前置条件

```
PRE 1: opportunity_id 在记忆库中存在且状态为 active
PRE 2: budget_limit > 0
PRE 3: 无同 opportunity_id 的进行中验证任务
```

## 后置条件

```
POST 1: 验证报告包含所有指定方法的执行结果
POST 2: decision 字段非空
POST 3: 预算消耗 <= budget_limit
POST 4: 结果写入记忆库，关联原 opportunity_id
```

---

## 验证方法详解

### 1. 🎯 着陆页烟雾测试（Landing Page Smoke Test）

```
流程：
  1. AI 自动生成着陆页（标题 + 价值主张 + 邮箱注册按钮）
  2. 部署到 Vercel/Cloudflare Pages（< 2 分钟）
  3. 投入 $20-$50 广告费引流
  4. 收集数据 3-7 天

衡量指标：
  - 页面访问量 (PV)
  - 邮箱注册转化率
  - 平均停留时间
  - 跳出率

通过标准：
  ✅ 注册转化率 > 3%
  ✅ 平均停留时间 > 30 秒
  ✅ 跳出率 < 70%
```

### 2. 📢 广告测试（Ad Campaign Test）

```
流程：
  1. AI 生成 3 组广告文案和素材
  2. 在 Google Ads / Meta Ads 创建测试计划
  3. 每组预算 $30-$70，总计 $100-$200
  4. 运行 3-5 天

衡量指标：
  - 点击率 (CTR)
  - 单次点击成本 (CPC)
  - 转化意向信号

通过标准：
  ✅ CTR > 1.5%
  ✅ CPC < 行业平均值 × 1.2
```

### 3. 📋 用户调研（Survey）

```
流程：
  1. AI 生成调研问卷（5-10 题）
  2. 通过 Typeform / 自建问卷部署
  3. AI 聊天机器人进行 1v1 深度访谈
  4. 目标样本量：50-200 人

关键问题：
  - 您目前如何解决这个问题？（验证痛点真实性）
  - 您每月愿意为此支付多少？（验证支付意愿）
  - 您对现有解决方案满意吗？（验证差异化空间）

通过标准：
  ✅ 有效回收率 > 20%
  ✅ 支付意愿 > 预估交付成本 × 3
  ✅ > 40% 受访者对现有方案不满意
```

### 4. 🔎 竞品分析（Competitor Analysis）

```
分析维度：
  - 竞品数量和规模（CRUNCHBASE / 企查查数据）
  - 用户评价分析（App Store / G2 / Trustpilot 爬取）
  - 定价策略分析
  - 功能对比矩阵
  - 差异化机会识别

输出：
  - 竞品清单（含收入估算、用户量、评分）
  - 功能差距热力图
  - 差异化策略建议
```

### 5. 🏗️ MVP 测试（Minimum Viable Product）

```
流程：
  1. AI 用 48 小时构建最简功能原型
  2. 部署到 50-100 名种子用户
  3. 收集使用数据和反馈

衡量指标：
  - 日活/周活 (DAU/WAU)
  - 核心功能使用率
  - NPS 净推荐值
  - 付费转化率

通过标准：
  ✅ WAU / 注册用户 > 30%
  ✅ 核心功能使用率 > 50%
  ✅ NPS > 20
```

---

## 决策矩阵

```
┌──────────┬───────────────────────────────────────────────┐
│  ✅ 继续  │ 愿意支付 > 交付成本 × 3                        │
│ PROCEED  │ 且 MVP 转化率 > 2%                             │
│          │ 且 竞品 < 10 或 差异化评分 > 0.7                │
├──────────┼───────────────────────────────────────────────┤
│  🔄 转向  │ 需求存在但：                                   │
│  PIVOT   │ - 目标群体不对                                  │
│          │ - 定价模型需要调整                               │
│          │ - 技术可行性存疑                                 │
│          │ → 修改参数后重新验证                             │
├──────────┼───────────────────────────────────────────────┤
│  📦 归档  │ 需求信号不足                                   │
│ ARCHIVE  │ 或 市场太小/萎缩                                │
│          │ 或 合规壁垒太高                                 │
│          │ → 关闭，经验写入记忆库                          │
└──────────┴───────────────────────────────────────────────┘
```

---

## 错误处理

| 错误码 | 描述 | 处理策略 |
|--------|------|---------|
| `BUDGET_EXCEEDED` | 预算消耗达到上限 | 停止所有付费验证方法，返回已有结果 |
| `NO_TRAFFIC` | 着陆页/广告无流量 | 延长观察期 3 天；仍无流量则标记 `inconclusive` |
| `SURVEY_LOW_RESPONSE` | 调研回收率 < 5% | 切换调研渠道或降低样本要求 |
| `MVP_BUILD_FAILURE` | MVP 构建失败 | 降级为仅着陆页验证，记录失败原因 |

---

## 监控指标

```
demand_validate_budget_utilization_ratio        # 预算利用率
demand_validate_methods_completed_total         # 完成的验证方法数
demand_validate_proceed_rate                    # 继续决策占比
demand_validate_archive_rate                    # 归档决策占比
demand_validate_duration_seconds                # 验证耗时
```
