# 🦞 OpenClaw 市场感知 — 扫描市场机会

扫描全网趋势，发现新兴需求、痛点和未被充分服务的市场机会。

## 参数

- `$ARGUMENTS` — 可选，指定目标行业领域（逗号分隔），如 `科技,SaaS`。留空则使用默认领域。

## 执行步骤

请按以下步骤执行市场感知分析：

### 第一阶段：数据采集

1. 使用 WebSearch 搜索以下关键词的最新趋势（根据用户指定的领域或默认领域 `科技, SaaS, 消费, 医疗健康`）：
   - "{领域} 新需求 市场机会 2026"
   - "{领域} 用户痛点 未解决问题"
   - "{领域} 创业机会 风口"
   - "{领域} 市场增长 新兴需求"
   - "{领域} 蓝海市场 细分赛道"

2. 使用 WebSearch 搜索国内平台和社区的最新动态：
   - "site:36kr.com {领域} 趋势"
   - "site:zhihu.com {领域} 痛点 需求"
   - "site:juejin.cn {领域} 新机会"
   - "{领域} site:itjuzi.com 投融资"
   - "{领域} 百度指数 增长"

### 第二阶段：噪声过滤

对采集到的信息进行过滤：
- 去除重复和推广内容
- 只保留与目标领域强相关的洞察
- 优先保留包含具体数据支撑的观点
- 优先引用36氪、艾瑞咨询、QuestMobile、IT桔子等权威来源

### 第三阶段：模式识别

识别以下模式：
- 📈 **上升趋势** — 搜索量/讨论量持续增长的趋势
- 🔥 **突发热点** — 短时间内讨论量爆发的主题
- 😤 **痛点聚集** — 同类抱怨/需求反复出现
- 🕳️ **市场空白** — 有需求但无满意解决方案
- ⚡ **技术拐点** — 新技术使原先不可能的事变为可能

### 第四阶段：机会评分

对每个识别到的机会按以下公式评分（0.00-1.00）：

```
score = market_size(0.25) + growth_rate(0.20) + competition_gap(0.20) + feasibility(0.15) + alignment(0.10) + timing(0.10)
```

评分等级：
- 0.00-0.30 🔴 低优先级 — 存档观察
- 0.30-0.60 🟡 中等优先级 — 列入候选
- 0.60-0.80 🟢 高优先级 — 建议执行 `/demand-validate`
- 0.80-1.00 🔥 紧急机会 — 立即验证

### 输出格式

请按以下 JSON 格式输出结果（嵌入 Markdown 代码块中）：

```json
{
  "scan_id": "scan_YYYY_MMDD_NNN",
  "timestamp": "当前时间",
  "config": {
    "domains": ["扫描的领域列表"],
    "depth": "medium"
  },
  "summary": {
    "sources_queried": 数量,
    "patterns_detected": 数量,
    "opportunities_generated": 数量
  },
  "opportunities": [
    {
      "id": "opp_YYYY_MMDD_NNN",
      "title": "机会标题",
      "domain": "所属领域",
      "pattern_type": "模式类型（上升/突发/痛点/空白/拐点）",
      "market_size_estimate": { "tam": "估算", "sam": "估算", "som": "估算" },
      "confidence_score": 0.00-1.00,
      "score_breakdown": {
        "market_size": 0.00,
        "growth_rate": 0.00,
        "competition_gap": 0.00,
        "feasibility": 0.00,
        "alignment": 0.00,
        "timing": 0.00
      },
      "key_evidence": ["证据1", "证据2", "证据3"],
      "recommended_next_steps": ["建议的下一步行动"]
    }
  ]
}
```

输出 JSON 后，请用中文总结本次扫描的 Top 3 机会，并给出行动建议。
