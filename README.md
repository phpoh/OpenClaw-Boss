# 🦞 让 OpenClaw 代替老板 / Let OpenClaw Replace Your Boss

**An Autonomous AI Entrepreneur Framework / 自主 AI 企业家框架**

> 💡 **核心思想：** 企业家最核心的能力是什么？——**敏锐发现市场需求，快速招人合作，交付产品赚钱。** 现在，OpenClaw（小龙虾）已经可以做到这一切——实时分析市场、权衡机遇、招募人才、实现盈利。你的下一个老板，可能是一只小龙虾。

> 💡 **Core Insight:** What is the most essential ability of an entrepreneur? — **Keenly discovering market needs, quickly recruiting collaborators, and delivering products for profit.** Now, OpenClaw can do all of this — analyzing markets in real-time, weighing opportunities, recruiting talent, and making money. Your next boss might be a lobster.

---

## 🔥 The Entrepreneurial DNA / 企业家基因

```
企业家做什么？What does an entrepreneur do?

    👀 发现需求          →  AI 实时扫描全网趋势、论坛、社交媒体
    Discover Needs          AI scans global trends, forums, social media in real-time

    ⚖️  权衡机遇          →  AI 数据驱动分析，计算 ROI 和风险
    Weigh Opportunities     AI analyzes data-driven ROI and risk calculations

    🏢 创建公司          →  AI 自动注册实体、开户、合规
    Form a Company          AI registers entities, opens accounts, ensures compliance

    👥 招人干活          →  AI 在 Upwork/Toptal 招募人类 + AI 协作者
    Recruit & Execute       AI hires humans + AI workers from freelance platforms

    💰 赚钱扩张          →  AI 管理财务、优化运营、持续增长
    Profit & Scale          AI manages finances, optimizes operations, sustains growth
```

---

## 📜 Whitepaper / 白皮书

- 🇬🇧 [English Whitepaper](./docs/WHITEPAPER_EN.md)
- 🇨🇳 [中文白皮书](./docs/WHITEPAPER_CN.md)

---

## 🧠 Core Concept / 核心概念

OpenClaw-Boss 通过 **Skills** 架构模拟企业家的完整决策链路：

```
👀 Market Sensing → ⚖️ Opportunity Validation → 🏢 Company Formation → 👥 Team Assembly → 🛠️ Product Delivery → 💰 Revenue & Growth
👀 市场感知     → ⚖️ 机会验证          → 🏢 公司组建      → 👥 团队搭建     → 🛠️ 产品交付     → 💰 收益与增长
```

---

## 🏗️ Architecture Overview / 架构概览

```
┌─────────────────────────────────────────────────────┐
│              🦞 OpenClaw-Boss Core                      │
│           (Autonomous Orchestrator / 自主编排引擎)     │
├──────────┬──────────┬──────────┬────────────────────┤
│ 👀Market │ 🏢Company│ 👥 Team  │ 🛠️ Product        │
│ Scanner  │ Builder  │ Assembler│ Delivery           │
│ 市场扫描  │ 公司构建  │ 团队组建  │ 产品交付           │
├──────────┴──────────┴──────────┴────────────────────┤
│             ⚙️ Skills Engine (技能引擎)                │
├─────────────────────────────────────────────────────┤
│        🧠 Foundation Models + Tool Use              │
│           基础模型 + 工具调用                          │
└─────────────────────────────────────────────────────┘
```

---

## 🛠️ Skills / 技能模块

| Skill | 🇬🇧 Description | 🇨🇳 说明 |
|-------|-------------|------|
| `market-sense` 👀 | Scan trends, forums, social media to discover unmet needs | 扫描趋势、论坛、社交媒体以发现未满足的需求 |
| `demand-validate` ⚖️ | Validate market demand through data analysis and MVP testing | 通过数据分析和 MVP 测试验证市场需求 |
| `company-form` 🏢 | Register company, set up legal and financial infrastructure | 注册公司，搭建法律和财务基础设施 |
| `team-assemble` 👥 | Recruit, interview, and onboard human/AI team members | 招募、面试并入职人类/AI 团队成员 |
| `product-build` 🛠️ | Design, develop, and deliver products or services | 设计、开发和交付产品或服务 |
| `finance-manage` 💰 | Manage revenue, expenses, payroll, and reinvestment | 管理收入、支出、薪资和再投资 |
| `growth-scale` 📈 | Optimize operations and scale the business | 优化运营并扩展业务 |

---

## 🚀 Quick Start / 快速使用

### 方式一：粘贴即用（推荐）

打开 [OPENCLAW_AGENT.md](./OPENCLAW_AGENT.md)，将文件**全部内容复制**，粘贴到任何 Claude 对话中即可激活小龙虾。无需安装、无需配置。

**支持的对话场景：**
- Claude Code 终端 / 桌面端 / IDE 插件
- claude.ai 网页对话
- Claude API 调用（作为 system prompt）
- 任何支持长文本输入的 AI 对话工具

**激活后直接说：**
```
扫描市场机会                    → 执行 market-sense
验证这个需求：AI简历优化服务     → 执行 demand-validate
完整创业流程                    → 从市场感知到增长扩展依次执行
```

> 粘贴后小龙虾会自动识别身份，你可以随时调用 7 个技能中的任意一个。

### 方式二：Claude Code 命令模式

如果你使用 Claude Code，可将 `.claude/commands/` 目录复制到项目根目录，然后用斜杠命令调用：

```
/market-sense technology,saas          # 扫描市场机会
/demand-validate AI简历优化服务         # 验证市场需求
/company-form 智简科技|有限责任公司|CN-SH|50000  # 组建公司
/team-assemble AI简历SaaS,$5000/月,8周  # 组建团队
/product-build AI简历优化SaaS功能规划    # 产品开发规划
/finance-manage 96000|72000|125000|80000 # 财务分析
/growth-scale SaaS项目月收$32000,+15%   # 增长策略
```

### 两种方式对比

| | 粘贴 OPENCLAW_AGENT.md | Claude Code 命令 |
|---|---|---|
| **适用场景** | 任何 Claude 对话 | Claude Code 项目内 |
| **使用方式** | 复制粘贴 | `/命令名` |
| **技能范围** | 7 个技能全部内置 | 每个命令独立调用 |
| **适合** | 快速体验、临时使用 | 长期项目、反复使用 |

---

## 📁 Repository Structure / 仓库结构

```
OpenClaw-Boss/
├── README.md
├── OPENCLAW_AGENT.md            # 🦞 一键激活 prompt（复制粘贴即用）
├── LICENSE
├── .claude/
│   └── commands/                # 🤖 Claude Code 可执行命令
│       ├── market-sense.md      # 👀 市场感知
│       ├── demand-validate.md   # ⚖️ 需求验证
│       ├── company-form.md      # 🏢 公司组建
│       ├── team-assemble.md     # 👥 团队组建
│       ├── product-build.md     # 🛠️ 产品构建
│       ├── finance-manage.md    # 💰 财务管理
│       └── growth-scale.md      # 📈 增长扩展
├── docs/
│   ├── WHITEPAPER_EN.md        # 🇬🇧 English whitepaper
│   └── WHITEPAPER_CN.md        # 🇨🇳 中文白皮书
├── skills/
│   ├── cn/                      # 🇨🇳 中文技能定义（设计规范）
│   │   ├── market-sense.md
│   │   ├── demand-validate.md
│   │   ├── company-form.md
│   │   ├── team-assemble.md
│   │   ├── product-build.md
│   │   ├── finance-manage.md
│   │   └── growth-scale.md
│   └── en/                      # 🇬🇧 English skill definitions (design spec)
│       ├── market-sense.md
│       ├── demand-validate.md
│       ├── company-form.md
│       ├── team-assemble.md
│       ├── product-build.md
│       ├── finance-manage.md
│       └── growth-scale.md
└── LICENSE
```

## ⭐ Star History

[![Star History Chart](https://api.star-history.com/svg?repos=phpoh/OpenClaw-Boss&type=Date)](https://star-history.com/#phpoh/OpenClaw-Boss&Date)

## 📜 License

MIT License - see [LICENSE](./LICENSE)

---

> 🦞 *"企业家最牛的地方，就是不睡觉也能赚钱。现在 OpenClaw 也可以了。"*
> *"The best entrepreneur is one who never sleeps, never biases, and never stops learning. That's OpenClaw."*
