# Skill: market-sense

> Scan digital ecosystems to discover unmet market needs
> 扫描数字生态系统以发现未满足的市场需求

## Definition

```yaml
skill:
  name: market-sense
  version: 0.1.0
  description: "Continuously scan the digital world to identify emerging needs, pain points, and underserved markets"
  trigger:
    - scheduled: "0 */6 * * *"
    - event: "market_shift_detected"
    - manual: true
  inputs:
    - target_domains: list[string]
    - depth: enum[shallow, medium, deep]
    - time_horizon: enum[week, month, quarter]
  outputs:
    - opportunity_report: OpportunityReport[]
    - confidence_score: float
    - recommended_actions: Action[]
  dependencies:
    - web-search
    - data-analysis
    - trend-modeling
```

## Methods / 方法

### Data Sources / 数据来源
- Social media platforms (Twitter/X, Reddit, HackerNews, ProductHunt, V2EX, 即刻)
- Search engine trend data (Google Trends, Baidu Index, 微信指数)
- Forum discussions and complaint mining (GitHub Issues, Stack Overflow, 知乎)
- Competitor analysis and market gap identification
- Patent filings and academic publications
- Government economic data and industry reports

### Analysis Pipeline / 分析流程
```
Raw Data → Noise Filter → Pattern Recognition → Opportunity Scoring → Report Generation
原始数据 → 噪声过滤 → 模式识别 → 机会评分 → 报告生成
```

### Opportunity Scoring Model / 机会评分模型
```
score = (
    market_size * 0.25 +
    growth_rate * 0.20 +
    competition_gap * 0.20 +
    feasibility * 0.15 +
    alignment * 0.10 +
    timing * 0.10
)
```

## Output Format / 输出格式

```json
{
  "opportunity_id": "opp_2026_0413_001",
  "title": "AI-Powered Resume Optimization for Non-English Speakers",
  "domain": "career_services",
  "market_size_estimate": "$2.3B",
  "confidence_score": 0.78,
  "key_evidence": [
    "Reddit r/jobs: 340% increase in 'resume help' posts from non-English speakers",
    "Google Trends: 'AI resume' searches up 580% YoY"
  ],
  "recommended_next_steps": ["demand-validate", "competitive-analysis"],
  "created_at": "2026-04-13T10:00:00Z"
}
```
