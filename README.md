# AI 替你当老板 / Let AI Be Your Boss

**An Autonomous AI Entrepreneur Framework / 自主 AI 企业家框架**

让 AI 具备企业家的核心能力——自主发现市场需求、创建公司、招募人才、交付产品并持续盈利。你的下一个老板，可能不是人。

Empowering AI with the core capabilities of an entrepreneur — autonomously discovering market demands, creating companies, recruiting talent, delivering products, and sustaining profitability. Your next boss might not be human.

---

## Whitepaper / 白皮书

- [English Whitepaper](./docs/WHITEPAPER_EN.md)
- [中文白皮书](./docs/WHITEPAPER_CN.md)

## Core Concept / 核心概念

AI-Founder 通过 **Skills** 架构模拟企业家的完整决策链路：

```
Market Sensing → Opportunity Validation → Company Formation → Team Assembly → Product Delivery → Revenue & Growth
市场感知 → 机会验证 → 公司组建 → 团队搭建 → 产品交付 → 收益与增长
```

## Architecture Overview / 架构概览

```
┌─────────────────────────────────────────────────┐
│                  AI-Founder Core                 │
│              (Autonomous Orchestrator)            │
│                自主编排引擎                        │
├──────────┬──────────┬──────────┬────────────────┤
│  Market  │ Company  │   Team   │    Product     │
│ Scanner  │ Builder  │ Assembler│   Delivery     │
│ 市场扫描  │ 公司构建 │ 团队组建  │   产品交付      │
├──────────┴──────────┴──────────┴────────────────┤
│              Skills Engine (技能引擎)              │
├─────────────────────────────────────────────────┤
│         Foundation Models + Tool Use             │
│            基础模型 + 工具调用                      │
└─────────────────────────────────────────────────┘
```

## Skills / 技能模块

| Skill | Description | 说明 |
|-------|-------------|------|
| `market-sense` | Scan trends, forums, social media to discover unmet needs | 扫描趋势、论坛、社交媒体以发现未满足的需求 |
| `demand-validate` | Validate market demand through data analysis and MVP testing | 通过数据分析和 MVP 测试验证市场需求 |
| `company-form` | Register company, set up legal and financial infrastructure | 注册公司，搭建法律和财务基础设施 |
| `team-assemble` | Recruit, interview, and onboard human/AI team members | 招募、面试并入职人类/AI 团队成员 |
| `product-build` | Design, develop, and deliver products or services | 设计、开发和交付产品或服务 |
| `finance-manage` | Manage revenue, expenses, payroll, and reinvestment | 管理收入、支出、薪资和再投资 |
| `growth-scale` | Optimize operations and scale the business | 优化运营并扩展业务 |

## Repository Structure / 仓库结构

```
AI-Founder/
├── README.md
├── docs/
│   ├── WHITEPAPER_EN.md        # English whitepaper
│   └── WHITEPAPER_CN.md        # 中文白皮书
├── skills/
│   ├── market-sense.md          # Market sensing skill definition
│   ├── demand-validate.md       # Demand validation skill definition
│   ├── company-form.md          # Company formation skill definition
│   ├── team-assemble.md         # Team assembly skill definition
│   ├── product-build.md         # Product building skill definition
│   ├── finance-manage.md        # Finance management skill definition
│   └── growth-scale.md          # Growth & scaling skill definition
└── LICENSE
```

## License

MIT License - see [LICENSE](./LICENSE)

---

> *"The best entrepreneur is one who never sleeps, never biases, and never stops learning. That's AI."*
> *"最好的企业家是那个从不睡觉、从无偏见、永不停止学习的存在。那就是 AI。"*
