# 🦞 OpenClaw 需求验证 — 验证市场机会是否真实

对已发现的市场机会进行低成本验证，判断需求是否真实、可变现。

## 参数

- `$ARGUMENTS` — 必填，指定要验证的机会描述（文本形式），如 "面向中小学生的AI错题本SaaS"。

## 执行步骤

请对以下市场机会进行全面的需求验证：**$ARGUMENTS**

### 第一阶段：竞品分析

1. 使用 WebSearch 搜索现有竞品：
   - "{机会描述} 竞品 现有方案"
   - "{机会描述} 同类产品 替代方案"
   - "{机会描述} 评测 对比"
   - 在企查查/天眼查搜索相关公司："{机会描述} 创业公司 融资"

2. 分析竞品格局：
   - 列出主要竞品（名称、网址、定价）
   - 分析用户评价（知乎、小红书、应用商店评论中的痛点、不满）
   - 识别差异化机会

### 第二阶段：市场需求数据验证

1. 使用 WebSearch 验证需求信号：
   - "{机会描述} 市场规模 行业报告"
   - "{机会描述} 增长趋势 百度指数"
   - "{机会描述} 用户需求 调研"

2. 寻找定量数据支撑：
   - 百度指数/微信指数搜索趋势
   - 知乎/小红书相关话题讨论热度
   - 艾瑞咨询/QuestMobile/易观分析相关行业报告数据
   - IT桔子相关赛道投融资数据

### 第三阶段：支付意愿评估

1. 使用 WebSearch 调研定价参考：
   - "{机会描述} 定价 收费标准"
   - "{机会描述} 用户愿意付多少钱"
   - "{机会描述} SaaS 定价方案"

2. 估算成本与收入：
   - 估算单用户交付成本（人民币）
   - 对比竞品定价区间
   - 计算支付意愿 / 交付成本 比值

### 第四阶段：可行性评估

1. 技术可行性（AI 能否在 48 小时内构建 MVP？）
2. 合规风险（是否涉及牌照、数据隐私、ICP备案等）
3. 资金需求（启动成本估算，人民币）

### 决策矩阵

根据验证结果给出 Go/No-Go 决策：

| 决策 | 条件 |
|------|------|
| ✅ **PROCEED（继续）** | 支付意愿 > 交付成本 × 3，且竞品 < 10 或差异化明显 |
| 🔄 **PIVOT（转向）** | 需求存在但目标群体/定价/技术路线需调整 |
| 📦 **ARCHIVE（归档）** | 需求信号不足，或市场太小，或合规壁垒太高 |

### 输出格式

请输出验证报告：

```json
{
  "opportunity": "验证的机会描述",
  "validation_methods": ["competitor_analysis", "market_data", "pricing_research"],
  "competitor_landscape": {
    "direct_competitors": [
      { "name": "名称", "url": "网址", "pricing": "定价", "strengths": "优势", "weaknesses": "劣势" }
    ],
    "differentiation_opportunities": ["差异化机会"]
  },
  "demand_signals": {
    "market_size_estimate": "估算",
    "growth_indicators": ["增长指标"],
    "user_pain_points": ["用户痛点"],
    "evidence_strength": "strong/moderate/weak"
  },
  "willingness_to_pay": {
    "estimated_delivery_cost": "交付成本估算",
    "competitor_price_range": "竞品价格区间",
    "wtcp_ratio": 支付意愿/交付成本比值,
    "verdict": "favorable/marginal/unfavorable"
  },
  "feasibility": {
    "technical": "high/medium/low",
    "regulatory_risk": "high/medium/low",
    "time_to_mvp": "估算时间",
    "startup_cost_estimate": "启动成本估算"
  },
  "decision": "proceed/pivot/archive",
  "decision_rationale": "决策理由（2-3句话）",
  "recommended_next_steps": ["建议的下一步行动"]
}
```

输出 JSON 后，请用中文总结验证结论和核心建议。
