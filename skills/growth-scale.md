# Skill: growth-scale

> Optimize operations and scale the business
> 优化运营并扩展业务

## Definition

```yaml
skill:
  name: growth-scale
  version: 0.1.0
  description: "Scale successful ventures, manage portfolio, and gracefully exit failures"
  trigger:
    - scheduled: "0 9 * * 1"    # Weekly growth review
    - event: "growth_milestone_reached"
    - event: "decline_detected"
    - manual: true
  inputs:
    - company_id: string
    - performance_data: PerformanceData
    - market_conditions: MarketData
  outputs:
    - growth_plan: GrowthPlan
    - portfolio_status: PortfolioStatus
    - strategic_recommendations: Recommendation[]
  dependencies:
    - finance-manage
```

## Growth Strategies / 增长策略

### 1. Optimize / 优化
- A/B test pricing, messaging, and user flows
- Reduce customer acquisition cost (CAC)
- Improve retention and reduce churn
- Automate manual processes

### 2. Expand / 扩展
- New customer segments
- Adjacent markets
- Geographic expansion
- Product line extensions

### 3. Scale / 规模化
- Infrastructure scaling
- Team expansion
- Process standardization
- Partnership development

### 4. Diversify / 多元化
- Launch new ventures using existing infrastructure
- Cross-leverage teams and technology
- Build a portfolio of complementary businesses

## Shutdown Protocol / 关闭协议

### Warning Signs / 预警信号
```
TRIGGER_REVIEW if:
  - monthly_burn > monthly_revenue × 2 for 2 months
  - customer_growth < 2% MoM
  - net_promoter_score < 20
  - key_team_members_depart
```

### Graceful Shutdown / 优雅关闭
```
1. Notify all stakeholders
2. Fulfill existing customer obligations
3. Settle all outstanding payments
4. Archive code, data, and documentation
5. Extract and preserve lessons learned
6. Redistribute team members to other ventures
7. Close legal entity properly
8. Final financial reconciliation
```

## Portfolio Management / 组合管理

### Multi-Venture Dashboard / 多项目仪表板

```
┌─────────────────────────────────────────────┐
│           OpenClaw-Boss Portfolio              │
│                                             │
│  Venture A ██████████░░ 80% target (Grow)   │
│  Venture B ██████░░░░░░ 55% target (Hold)   │
│  Venture C ██░░░░░░░░░░ 20% target (Review) │
│  Venture D ░░░░░░░░░░░░  5% target (Exit)   │
│                                             │
│  Total Revenue: $XXX,XXX                    │
│  Active Ventures: 4                         │
│  Portfolio ROI: XX%                         │
└─────────────────────────────────────────────┘
```

### Resource Allocation / 资源分配
- Reinvest profits from high-performing ventures
- Redirect resources from declining ventures
- Balance portfolio risk across industries
- Maintain reserve fund for new opportunities
