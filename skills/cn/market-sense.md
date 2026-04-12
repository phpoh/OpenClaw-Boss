# 🦞 Skill: market-sense（市场感知）

> **🇨🇳 中文版** | [🇬🇧 English](../en/market-sense.md)

---

## 元信息

| 字段 | 值 |
|------|-----|
| 名称 | `market-sense` |
| 版本 | `1.0.0` |
| 负责组件 | 🧭 规划器（Planner） |
| 执行者 | ⚡ 执行器（Executor） |
| 最小执行间隔 | 3600 秒（1 小时） |
| 超时限制 | 1800 秒（30 分钟） |
| 最大重试次数 | 3 |

---

## 概述

OpenClaw 的"嗅觉"。企业家最核心的能力就是**敏锐发现市场需求**，本技能模拟这一能力：持续扫描全网数据源，识别新兴需求、痛点和未被充分服务的市场。

---

## 严格定义

```yaml
skill:
  name: market-sense
  version: 1.0.0
  description: "持续扫描数字世界，识别新兴需求、痛点和未被充分服务的市场"
  trigger:
    - scheduled: "0 */4 * * *"          # 每 4 小时自动执行
    - event: "market_shift_detected"    # 外部事件触发
    - event: "competitor_launch"        # 竞品发布触发
    - manual: true                      # 允许手动触发
  inputs:
    - name: target_domains
      type: "list[string]"
      required: false
      default: ["technology", "saas", "consumer", "healthcare"]
      description: "目标行业领域列表"
    - name: depth
      type: "enum[shallow, medium, deep]"
      required: false
      default: "medium"
      description: "扫描深度：浅层（快扫）、中等（标准）、深层（全面分析）"
    - name: time_horizon
      type: "enum[week, month, quarter]"
      required: false
      default: "month"
      description: "趋势观察时间窗口"
    - name: max_opportunities
      type: "int"
      required: false
      default: 20
      constraints: "1 <= value <= 100"
      description: "返回的最大机会数量"
  outputs:
    - name: opportunities
      type: "list[Opportunity]"
      description: "按优先级排序的市场机会列表"
    - name: scan_summary
      type: "ScanSummary"
      description: "本次扫描的统计摘要"
    - name: confidence_scores
      type: "list[float]"
      constraints: "0.0 <= value <= 1.0"
      description: "每个机会的置信度评分"
  dependencies:
    skills: []
    tools:
      - web_search          # 网页搜索 API
      - api_client          # 通用 HTTP 客户端
      - database_query      # 数据库查询
      - statistical_analysis # 统计分析引擎
      - nlp_processor       # 自然语言处理
    data_sources:
      - twitter_api
      - reddit_api
      - hackernews_api
      - producthunt_api
      - google_trends
      - baidu_index
      - github_events
```

---

## 前置条件（Pre-conditions）

```
PRE 1: 至少有 1 个数据源 API 可达
PRE 2: 网络连接正常（延迟 < 5000ms）
PRE 3: 上次扫描完成时间距今 > 最小执行间隔
PRE 4: 数据库连接池有空闲连接
```

---

## 后置条件（Post-conditions）

```
POST 1: 输出 opportunities 列表长度 <= max_opportunities
POST 2: 每个机会的 confidence_score ∈ [0.0, 1.0]
POST 3: 所有机会按 score 降序排列
POST 4: 扫描结果已持久化到记忆库
POST 5: 执行日志已记录，包含耗时和来源统计
```

---

## 执行流程

### 第一阶段：数据采集

```
┌──────────────────────────────────────────────────────────┐
│                    📡 数据采集层                           │
├──────────────┬──────────────┬──────────────┬─────────────┤
│  社交媒体     │   搜索引擎    │   技术社区    │   商业数据   │
│  Twitter/X   │ Google Trends│ HackerNews   │ SEC EDGAR   │
│  Reddit      │ 百度指数      │ ProductHunt  │ Crunchbase  │
│  即刻        │ 微信指数      │ V2EX         │ 企查查       │
│  微博        │ 淘宝热搜      │ GitHub       │ 天眼查       │
└──────┬───────┴──────┬───────┴──────┬───────┴──────┬──────┘
       │              │              │              │
       └──────────────┴──────┬───────┴──────────────┘
                             ▼
                    ┌─────────────────┐
                    │   原始数据池      │
                    │  Raw Data Pool  │
                    └────────┬────────┘
```

**数据采集规则：**
- 每个数据源设置独立超时（10 秒）
- 单次采集总量上限 10000 条原始记录
- 采集失败的数据源跳过，不阻塞整体流程
- 所有采集记录包含时间戳和来源标记

### 第二阶段：噪声过滤

```python
# 伪代码：噪声过滤规则
def noise_filter(raw_data):
    filtered = []
    for item in raw_data:
        # 规则 1：去除重复内容（SimHash 去重，阈值 0.95）
        if is_duplicate(item, filtered, threshold=0.95):
            continue
        # 规则 2：去除广告和推广内容
        if is_promotional(item):
            continue
        # 规则 3：去除低质量内容（字数 < 10 或无意义）
        if is_low_quality(item):
            continue
        # 规则 4：保留与目标领域相关的内容
        if is_relevant(item, target_domains):
            filtered.append(item)
    return filtered
```

### 第三阶段：模式识别

```
识别模式类型：
  📈 上升趋势    — 搜索量/讨论量持续增长（连续 7 天增长 > 10%/天）
  🔥 突发热点    — 短时间内讨论量爆发（24h 增长 > 300%）
  😤 痛点聚集    — 同类抱怨/需求反复出现（跨平台出现 > 5 次）
  🕳️ 市场空白    — 有需求但无 satisfactory 解决方案
  ⚡ 技术拐点    — 新技术使原先不可能的事变为可能
```

### 第四阶段：机会评分

```
score = (
    market_size     × 0.25   +  # 市场规模（TAM 估算）
    growth_rate     × 0.20   +  # 增长速率
    competition_gap × 0.20   +  # 竞争空白程度
    feasibility     × 0.15   +  # 技术可行性
    alignment       × 0.10   +  # 与现有能力匹配度
    timing          × 0.10      # 时机评分（是否窗口期）
)

评分标准：
  0.00 - 0.30  🔴 低优先级 — 存档观察
  0.30 - 0.60  🟡 中等优先级 — 列入候选，定期复查
  0.60 - 0.80  🟢 高优先级 — 触发 demand-validate 技能
  0.80 - 1.00  🔥 紧急机会 — 立即触发验证 + 通知人类
```

---

## 输出格式

```json
{
  "scan_id": "scan_2026_0413_001",
  "timestamp": "2026-04-13T10:00:00Z",
  "config": {
    "depth": "medium",
    "time_horizon": "month",
    "domains": ["technology", "saas"]
  },
  "summary": {
    "sources_queried": 12,
    "sources_failed": 1,
    "raw_items_collected": 8734,
    "after_filtering": 1256,
    "patterns_detected": 15,
    "opportunities_generated": 8
  },
  "opportunities": [
    {
      "id": "opp_2026_0413_001",
      "title": "面向非英语母语者的 AI 简历优化服务",
      "domain": "career_services",
      "pattern_type": "😤 pain_point",
      "market_size_estimate": {
        "tam": "$2.3B",
        "sam": "$800M",
        "som": "$50M"
      },
      "confidence_score": 0.78,
      "score_breakdown": {
        "market_size": 0.72,
        "growth_rate": 0.85,
        "competition_gap": 0.80,
        "feasibility": 0.90,
        "alignment": 0.65,
        "timing": 0.70
      },
      "key_evidence": [
        "Reddit r/jobs: 非英语用户 'resume help' 帖子增长 340%",
        "Google Trends: 'AI resume' 搜索量同比增长 580%",
        "ProductHunt: 近 30 天上线 3 款竞品但均不支持多语言"
      ],
      "recommended_next_steps": [
        {"skill": "demand-validate", "priority": "high"},
        {"skill": "market-sense", "args": {"depth": "deep", "target_domains": ["career_services"]}}
      ],
      "created_at": "2026-04-13T10:15:00Z"
    }
  ],
  "execution_meta": {
    "duration_seconds": 342,
    "tokens_used": 15420,
    "cost_usd": 0.12
  }
}
```

---

## 错误处理

| 错误码 | 描述 | 处理策略 |
|--------|------|---------|
| `TIMEOUT` | 单次执行超过 1800 秒 | 返回已完成的部分结果，标记为 `partial` |
| `SOURCE_UNAVAILABLE` | 数据源 API 不可达 | 跳过该源，继续其他源；连续 3 次失败则告警 |
| `RATE_LIMITED` | API 调用频率受限 | 指数退避重试，最长等待 60 秒 |
| `QUOTA_EXCEEDED` | Token/调用配额用尽 | 立即停止，通知人类，记录已收集数据 |
| `DATA_CORRUPT` | 返回数据格式异常 | 丢弃异常数据，记录日志继续执行 |

---

## 监控指标

```
# 执行健康度
market_sense_execution_duration_seconds      # 执行耗时（直方图）
market_sense_sources_healthy_ratio           # 健康数据源比例（0.0-1.0）
market_sense_opportunities_found_total       # 发现的机会总数（计数器）

# 质量指标
market_sense_false_positive_rate             # 误报率（后续验证失败/总机会）
market_sense_coverage_score                  # 覆盖率（有效数据源/总数据源）
```

---

## 人类审批点

```
无需人类审批即可执行 ✅
但以下情况需通知人类 📢：
  - 发现 confidence_score > 0.85 的紧急机会
  - 连续 3 次扫描无任何新发现
  - 数据源不可用比例 > 50%
  - 单次执行成本 > $1
```
