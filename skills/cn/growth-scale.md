# 🦞 Skill: growth-scale（增长与扩展）

> **🇨🇳 中文版** | [🇬🇧 English](../en/growth-scale.md)

---

## 元信息

| 字段 | 值 |
|------|-----|
| 名称 | `growth-scale` |
| 版本 | `1.0.0` |
| 负责组件 | 🧭 规划器（Planner）+ 🪞 反思器（Reflector） |
| 超时限制 | 3600 秒（1 小时，单次审查） |
| 最大重试次数 | 1 |
| 上游依赖 | `finance-manage` |

---

## 概述

企业家的"做大做强"能力。产品上线了、开始赚钱了——然后呢？OpenClaw 决定是继续投入增长、保持现状、还是优雅退出。同时管理多个创业项目的组合，像真正的投资老板一样。

---

## 严格定义

```yaml
skill:
  name: growth-scale
  version: 1.0.0
  description: "扩展成功的业务，管理项目组合，优雅退出失败项目"
  trigger:
    - scheduled: "0 9 * * 1"              # 每周一 9:00 增长审查
    - event: "growth_milestone_reached"    # 达到增长里程碑
    - event: "decline_detected"            # 检测到衰退信号
    - event: "runway_below_threshold"      # 资金跑道不足
    - manual: true
  inputs:
    - name: company_id
      type: "string"
      required: true
    - name: performance_data
      type: "PerformanceData"
      required: true
      description: "当前绩效数据（来自 finance-manage 和 product-build）"
    - name: market_conditions
      type: "MarketData"
      required: false
      description: "当前市场环境数据"
  outputs:
    - name: growth_plan
      type: "GrowthPlan"
      description: "增长计划"
    - name: venture_status
      type: "enum[grow, hold, review, exit]"
      description: "项目状态决策"
    - name: strategic_recommendations
      type: "list[Recommendation]"
      description: "战略建议列表"
  dependencies:
    skills: ["finance-manage"]
    tools:
      - analytics_engine        # 分析引擎
      - ab_test_platform        # A/B 测试平台
      - marketing_automation    # 营销自动化
      - market_data_feed        # 市场数据源
```

---

## 项目状态决策矩阵

```
┌──────────┬───────────────────────────────────────────────────┐
│ 📈 GROW  │ 月收入增长 > 10% MoM                              │
│          │ 且 LTV/CAC > 3                                    │
│          │ 且资金跑道 > 6 个月                                │
│          │ → 加大投入，扩张团队，拓展市场                     │
├──────────┼───────────────────────────────────────────────────┤
│ ⏸️ HOLD  │ 月收入增长 0-10% MoM                              │
│          │ 且 LTV/CAC > 1.5                                  │
│          │ 且资金跑道 > 6 个月                                │
│          │ → 保持投入，优化效率，寻找增长点                   │
├──────────┼───────────────────────────────────────────────────┤
│ 🔍 REVIEW│ 连续 2 个月负增长                                 │
│          │ 或 LTV/CAC < 1.5                                  │
│          │ 或资金跑道 < 6 个月                                │
│          │ → 深度诊断，制定转型方案                           │
├──────────┼───────────────────────────────────────────────────┤
│ 🚪 EXIT  │ 连续 3 个月：burn > revenue × 2                   │
│          │ 且客户增长 < 5% MoM                               │
│          │ 且 6 个月内无盈利路径                              │
│          │ → 启动优雅退出流程                                │
└──────────┴───────────────────────────────────────────────────┘
```

---

## 增长策略工具箱

### 🎯 优化（Optimize）

```
可自动执行：
  [ ] A/B 测试定价方案（3 个价格点）
  [ ] 优化着陆页转化率（标题、CTA、配色）
  [ ] 邮件营销自动化流程优化
  [ ] 客户成功流程标准化
  [ ] 自动化运营流程（减少人力成本）

衡量指标：
  - 转化率提升幅度
  - CAC 降低幅度
  - 流失率降低幅度
```

### 🌍 扩展（Expand）

```
需要规划 + 🛡️ 人类审批：
  [ ] 新客户群体定位
  [ ] 相邻市场进入评估
  [ ] 地理扩张可行性分析
  [ ] 产品线扩展规划
  [ ] 合作伙伴/渠道拓展

每个扩展动作需要：
  1. 市场调研报告（自动生成）
  2. 投入产出预测
  3. 风险评估
  4. 🛡️ 人类审批
```

### ⚙️ 规模化（Scale）

```
  [ ] 基础设施扩容（自动弹性伸缩）
  [ ] 团队扩展（触发 team-assemble）
  [ ] 流程标准化文档（SOP）
  [ ] 客户支持体系升级
  [ ] 合规体系完善
```

### 🔄 多元化（Diversify）

```
利用现有基础设施启动新项目：
  [ ] 识别可复用的技术/团队/客户资源
  [ ] 生成新项目机会评估
  [ ] 触发 market-sense → demand-validate 全流程
  [ ] 🛡️ 人类审批新项目启动
```

---

## 优雅退出协议

```
退出流程（预计 2-4 周）：

第 1 周：通知与评估
  [ ] 通知所有利益相关方（人类团队、客户、供应商）
  [ ] 评估未完成合同和承诺
  [ ] 生成退出成本估算

第 2 周：有序收尾
  [ ] 完成所有客户未交付义务
  [ ] 结算所有应付款项
  [ ] 通知 AI 工作者停止工作

第 3 周：资产处置
  [ ] 归档代码、数据、文档
  [ ] 提取经验教训写入记忆库
  [ ] 重新分配人类团队成员
  [ ] 评估资产出售可能性

第 4 周：最终清算
  [ ] 关闭法律实体
  [ ] 最终财务对账
  [ ] 税务清算
  [ ] 生成项目复盘报告
```

---

## 多项目组合管理

```
┌─────────────────────────────────────────────────────────────┐
│                  🦞 OpenClaw-Boss 项目组合                    │
├──────────────┬──────────┬──────────┬──────────┬─────────────┤
│ 项目          │ 状态     │ 收入/月   │ 增长率    │ 决策        │
├──────────────┼──────────┼──────────┼──────────┼─────────────┤
│ 项目 A (SaaS) │ 📈 Grow  │ $32,000  │ +15% MoM │ 加大投入     │
│ 项目 B (API)  │ ⏸️ Hold  │ $12,000  │ +3% MoM  │ 优化效率     │
│ 项目 C (App)  │ 🔍 Review│ $4,000   │ -5% MoM  │ 诊断转型     │
│ 项目 D (Shop) │ 🚪 Exit  │ $800     │ -20% MoM │ 退出清算     │
├──────────────┼──────────┼──────────┼──────────┼─────────────┤
│ 组合合计      │          │ $48,800  │ +8% MoM  │             │
│ 总燃烧率     │          │ $38,000  │          │             │
│ 组合跑道     │          │ 7.2 月   │          │             │
└──────────────┴──────────┴──────────┴──────────┴─────────────┘

资源分配规则：
  - GROW 项目获得 60% 新增资源
  - HOLD 项目获得 25% 新增资源
  - REVIEW 项目获得 15% 新增资源
  - EXIT 项目不再获得新资源，逐步释放
```

---

## 错误处理

| 错误码 | 描述 | 处理策略 |
|--------|------|---------|
| `MARKET_CRASH` | 市场剧烈波动 | 立即冻结所有扩张计划，通知人类 |
| `COMPETITOR_DISRUPTION` | 竞品重大发布 | 触发紧急 market-sense 深度扫描 |
| `KEY_PERSON_LEFT` | 关键人员离职 | 触发 team-assemble 紧急招聘 |
| `REGULATION_CHANGE` | 监管政策变化 | 🛑 暂停运营，人类评估合规影响 |
| `DATA_BREACH` | 数据泄露 | 立即隔离受影响系统，通知人类，启动应急流程 |

---

## 监控指标

```
growth_scale_portfolio_revenue_usd             # 组合总收入
growth_scale_venture_count_by_status           # 各状态项目数
growth_scale_exit_success_rate                 # 退出成功率
growth_scale_resource_allocation_efficiency    # 资源分配效率
growth_scale_time_to_growth_decision_seconds   # 增长决策耗时
```
