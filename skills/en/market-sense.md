# 🦞 Skill: market-sense

> **🇬🇧 English** | [🇨🇳 中文版](../cn/market-sense.md)

---

## Metadata

| Field | Value |
|-------|-------|
| Name | `market-sense` |
| Version | `1.0.0` |
| Owner | 🧭 Planner |
| Executor | ⚡ Executor |
| Min Execution Interval | 3600s (1 hour) |
| Timeout | 1800s (30 minutes) |
| Max Retries | 3 |

---

## Overview

OpenClaw's "instinct." The most essential ability of an entrepreneur is **keenly discovering market needs**. This skill simulates that ability: continuously scanning all web data sources to identify emerging needs, pain points, and underserved markets.

---

## Strict Definition

```yaml
skill:
  name: market-sense
  version: 1.0.0
  description: "Continuously scan the digital world to identify emerging needs, pain points, and underserved markets"
  trigger:
    - scheduled: "0 */4 * * *"          # Every 4 hours
    - event: "market_shift_detected"    # External event trigger
    - event: "competitor_launch"        # Competitor launch trigger
    - manual: true                      # Allow manual trigger
  inputs:
    - name: target_domains
      type: "list[string]"
      required: false
      default: ["technology", "saas", "consumer", "healthcare"]
      description: "Target industry domains to scan"
    - name: depth
      type: "enum[shallow, medium, deep]"
      required: false
      default: "medium"
      description: "Scan depth: shallow (quick), medium (standard), deep (comprehensive)"
    - name: time_horizon
      type: "enum[week, month, quarter]"
      required: false
      default: "month"
      description: "Trend observation time window"
    - name: max_opportunities
      type: "int"
      required: false
      default: 20
      constraints: "1 <= value <= 100"
      description: "Maximum number of opportunities to return"
  outputs:
    - name: opportunities
      type: "list[Opportunity]"
      description: "Prioritized list of market opportunities"
    - name: scan_summary
      type: "ScanSummary"
      description: "Statistical summary of this scan"
    - name: confidence_scores
      type: "list[float]"
      constraints: "0.0 <= value <= 1.0"
      description: "Confidence score for each opportunity"
  dependencies:
    skills: []
    tools:
      - web_search
      - api_client
      - database_query
      - statistical_analysis
      - nlp_processor
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

## Pre-conditions

```
PRE 1: At least 1 data source API is reachable
PRE 2: Network connectivity is healthy (latency < 5000ms)
PRE 3: Time since last scan > min execution interval
PRE 4: Database connection pool has available connections
```

---

## Post-conditions

```
POST 1: Output opportunities list length <= max_opportunities
POST 2: Each opportunity's confidence_score ∈ [0.0, 1.0]
POST 3: All opportunities sorted by score descending
POST 4: Scan results persisted to Memory store
POST 5: Execution log recorded with duration and source statistics
```

---

## Execution Flow

### Phase 1: Data Collection

```
┌──────────────────────────────────────────────────────────┐
│                    📡 Data Collection Layer                │
├──────────────┬──────────────┬──────────────┬─────────────┤
│  Social Media │  Search Eng. │  Tech Comm.  │  Biz Data   │
│  Twitter/X   │ Google Trends│ HackerNews   │ SEC EDGAR   │
│  Reddit      │ Baidu Index  │ ProductHunt  │ Crunchbase  │
│  Jike        │ WeChat Index │ V2EX         │ Qichacha    │
│  Weibo       │ Taobao Hot   │ GitHub       │ Tianyancha  │
└──────┬───────┴──────┬───────┴──────┬───────┴──────┬──────┘
       │              │              │              │
       └──────────────┴──────┬───────┴──────────────┘
                             ▼
                    ┌─────────────────┐
                    │  Raw Data Pool  │
                    └────────┬────────┘
```

**Collection Rules:**
- Each data source has independent timeout (10s)
- Single collection cap: 10000 raw records
- Failed sources are skipped, not blocking
- All records include timestamp and source tag

### Phase 2: Noise Filtering

```python
# Pseudocode: Noise filtering rules
def noise_filter(raw_data):
    filtered = []
    for item in raw_data:
        # Rule 1: Deduplicate (SimHash, threshold 0.95)
        if is_duplicate(item, filtered, threshold=0.95):
            continue
        # Rule 2: Remove ads and promotional content
        if is_promotional(item):
            continue
        # Rule 3: Remove low-quality content (< 10 chars or meaningless)
        if is_low_quality(item):
            continue
        # Rule 4: Keep only domain-relevant content
        if is_relevant(item, target_domains):
            filtered.append(item)
    return filtered
```

### Phase 3: Pattern Recognition

```
Pattern types to detect:
  📈 Rising Trend   — Sustained search/discussion growth (> 10%/day for 7 days)
  🔥 Sudden Spike   — Discussion explosion (> 300% increase in 24h)
  😤 Pain Cluster   — Same complaint/need repeated (> 5 times cross-platform)
  🕳️ Market Gap     — Demand exists but no satisfactory solution
  ⚡ Tech Inflection — New tech makes previously impossible things possible
```

### Phase 4: Opportunity Scoring

```
score = (
    market_size     × 0.25   +  # Market size (TAM estimate)
    growth_rate     × 0.20   +  # Growth rate
    competition_gap × 0.20   +  # Competition gap level
    feasibility     × 0.15   +  # Technical feasibility
    alignment       × 0.10   +  # Alignment with existing capabilities
    timing          × 0.10      # Timing score (window of opportunity)
)

Scoring thresholds:
  0.00 - 0.30  🔴 Low priority    — Archive for observation
  0.30 - 0.60  🟡 Medium priority — Add to candidates, periodic review
  0.60 - 0.80  🟢 High priority   — Trigger demand-validate skill
  0.80 - 1.00  🔥 Urgent          — Immediate validation + notify human
```

---

## Output Format

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
      "title": "AI-Powered Resume Optimization for Non-English Speakers",
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
        "Reddit r/jobs: 340% increase in 'resume help' posts from non-English speakers",
        "Google Trends: 'AI resume' searches up 580% YoY",
        "ProductHunt: 3 competitors launched in 30 days, none support multilingual"
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

## Error Handling

| Error Code | Description | Recovery Strategy |
|------------|-------------|-------------------|
| `TIMEOUT` | Execution exceeds 1800s | Return partial results, flag as `partial` |
| `SOURCE_UNAVAILABLE` | Data source API unreachable | Skip source, continue others; alert after 3 consecutive failures |
| `RATE_LIMITED` | API rate limit hit | Exponential backoff, max wait 60s |
| `QUOTA_EXCEEDED` | Token/call quota exhausted | Stop immediately, notify human, save collected data |
| `DATA_CORRUPT` | Unexpected response format | Discard corrupt data, log and continue |

---

## Monitoring Metrics

```
# Execution health
market_sense_execution_duration_seconds      # Duration histogram
market_sense_sources_healthy_ratio           # Healthy source ratio (0.0-1.0)
market_sense_opportunities_found_total       # Total opportunities found (counter)

# Quality metrics
market_sense_false_positive_rate             # False positive rate (failed validations / total)
market_sense_coverage_score                  # Coverage (healthy sources / total sources)
```

---

## Human Approval Points

```
No human approval required to execute ✅
Notify human when 📢:
  - Opportunity with confidence_score > 0.85 discovered
  - 3 consecutive scans yield no new findings
  - > 50% of data sources unavailable
  - Single execution cost > $1
```
