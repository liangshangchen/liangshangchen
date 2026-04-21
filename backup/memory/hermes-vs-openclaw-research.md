# OpenClaw vs Hermes Agent 深度调研报告

> 调研时间：2026-04-15
> 调研工具：GitHub API、DuckDuckGo 搜索、第三方对比文章

---

## 1. 基本信息概览

| 维度 | OpenClaw | Hermes Agent |
|------|----------|-------------|
| **GitHub** | [openclaw/openclaw](https://github.com/openclaw/openclaw) | [NousResearch/hermes-agent](https://github.com/nousresearch/hermes-agent) |
| **Stars** | ~357,200 | ~83,500 |
| **Forks** | 未精确获取 | ~11,200 |
| **语言** | TypeScript/Node.js | Python |
| **许可证** | MIT | MIT |
| **创建时间** | 2025 年中 | 2025-07-22 |
| **最后更新** | 2026-04-14 | 2026-04-14 |
| **官网** | [openclaw.ai](https://openclaw.ai) / [docs.openclaw.ai](https://docs.openclaw.ai) | [hermes-agent.nousresearch.com](https://hermes-agent.nousresearch.com) |
| **开发团队** | OpenClaw 社区 | [Nous Research](https://nousresearch.com) |
| **赞助商** | OpenAI、GitHub、NVIDIA、Vercel 等 | — |

**来源**: GitHub API 实时查询 (2026-04-15)

---

## 2. 架构差异

### OpenClaw
- **Gateway 控制面**：单进程 WebSocket 网关（默认端口 18789），负责会话管理、频道路由、工具调度
- **Pi Agent Runtime**：基于 RPC 的 agent 运行时，支持工具流和块流
- **多 Agent 路由**：可将不同频道/账号/联系人路由到隔离的 agent 实例（独立 workspace + session）
- **Session 模型**：main session（直接对话）+ group isolation + activation modes
- **Node.js 全栈**：TypeScript 编写，npm 分发

**来源**: [OpenClaw README](https://github.com/openclaw/openclaw) + [docs.openclaw.ai](https://docs.openclaw.ai)

### Hermes Agent
- **Gateway 进程**：单一网关进程同时处理所有消息平台
- **六种终端后端**：local、Docker、SSH、Daytona、Singularity、Modal
- **子 Agent 并行**：可生成隔离的 subagent 并行执行任务，支持 Python 脚本通过 RPC 调用工具
- **Serverless 持久化**：Daytona 和 Modal 提供无服务器持久化，空闲时休眠，按需唤醒
- **Python 全栈**：Python 3.11+，uv 包管理

**来源**: [Hermes Agent README](https://github.com/nousresearch/hermes-agent)

### 架构对比总结

| 特性 | OpenClaw | Hermes Agent |
|------|----------|-------------|
| 核心语言 | TypeScript | Python |
| 运行时 | Node.js 22/24 | Python 3.11+ |
| 分发方式 | npm | pip/uv |
| 无服务器支持 | 无（需自建 VPS） | Modal/Daytona 原生支持 |
| 多 Agent | 路由式隔离 | 子 Agent 并行执行 |
| 研究/训练集成 | 无 | 内置 Atropos RL 环境 |

---

## 3. 记忆系统对比

### OpenClaw
- **文件式记忆**：MEMORY.md（长期记忆）+ memory/YYYY-MM-DD.md（每日笔记）
- **语义搜索**：`memory_search` 工具，基于语义匹配查找记忆
- **memory_get**：读取特定记忆文件
- **Active Memory**（2026.4 新增）：主动记忆治理，Agent 可自主决定记忆存留
- **Context Compaction**：会话上下文压缩，防止超长对话溢出
- **来源**: [docs.openclaw.ai/concepts/memory](https://docs.openclaw.ai/concepts/memory)

### Hermes Agent
- **Agent 策展记忆**：Agent 自主管理记忆，定期 nudge 提醒持久化知识
- **FTS5 全文搜索**：会话历史全文索引，LLM 摘要辅助跨会话召回
- **Honcho 方言建模**：基于 [plastic-labs/honcho](https://github.com/plastic-labs/honcho) 的用户画像建模
- **自主 Skill 创建**：复杂任务后自动创建技能（经验→能力转化）
- **来源**: [Hermes README](https://github.com/nousresearch/hermes-agent)

### 记忆系统对比

| 特性 | OpenClaw | Hermes Agent |
|------|----------|-------------|
| 存储方式 | 文件系统（Markdown） | 文件系统 + FTS5 索引 |
| 语义搜索 | ✅ | ✅（FTS5 + LLM 摘要） |
| 主动记忆治理 | ✅（Active Memory） | ✅（nudge 机制） |
| 用户画像建模 | ❌ | ✅（Honcho） |
| 经验→技能自动转化 | ❌ | ✅ |
| 跨会话召回 | 手动（AGENTS.md 指引） | 自动（FTS5 + LLM） |

---

## 4. Skill 系统对比

### OpenClaw
- **三层 Skill**：bundled（内置）、managed（ClawHub 托管）、workspace（本地工作区）
- **SKILL.md 规范**：每个 skill 目录包含 SKILL.md 定义触发条件和指令
- **ClawHub 市场**：[clawhub.com](https://clawhub.com) 技能市场，支持搜索、安装、更新、发布
- **Skill Vetter**：安装前安全审计工具
- **来源**: [docs.openclaw.ai/tools/skills](https://docs.openclaw.ai/tools/skills)

### Hermes Agent
- **过程性记忆**：技能即 Agent 的过程性记忆，通过使用自我改进
- **Skills Hub**：[agentskills.io](https://agentskills.io) 开放标准技能库
- **自主创建**：复杂任务后自动生成技能
- **AgentSkills 兼容**：兼容 agentskills.io 开放标准
- **来源**: [Hermes README](https://github.com/nousresearch/hermes-agent)

### Skill 系统对比

| 特性 | OpenClaw | Hermes Agent |
|------|----------|-------------|
| 技能市场 | ✅ ClawHub | ✅ agentskills.io |
| 标准化 | ✅ SKILL.md 规范 | ✅ agentskills.io 标准 |
| 安装前审计 | ✅ Skill Vetter | 未提及 |
| 自动创建技能 | ❌ | ✅ |
| 技能自我改进 | ❌ | ✅（使用中迭代） |

---

## 5. 多平台接入能力

### OpenClaw（25+ 平台）
WhatsApp、Telegram、Slack、Discord、Google Chat、Signal、iMessage (BlueBubbles)、IRC、Microsoft Teams、Matrix、**Feishu**、LINE、Mattermost、Nextcloud Talk、Nostr、Synology Chat、Tlon、Twitch、Zalo、WeChat、WebChat、macOS、iOS、Android

### Hermes Agent（6 平台）
Telegram、Discord、Slack、WhatsApp、Signal、Email + CLI

**结论**：OpenClaw 在平台覆盖面上远超 Hermes Agent，尤其在中文生态（飞书、微信、Zalo）方面有独特优势。

**来源**: 两者 README

---

## 6. 安全模型

### OpenClaw
- **DM 配对机制**：未知发送者需配对码验证（dmPolicy="pairing"）
- **Allowlist**：基于频道的允许列表
- **命令审批**：危险操作需用户确认
- **Doctor 诊断**：`openclaw doctor` 检测不安全的 DM 策略
- **来源**: [docs.openclaw.ai/gateway/security](https://docs.openclaw.ai/gateway/security)

### Hermes Agent
- **命令审批**：危险命令需用户确认
- **DM 配对**：类似 OpenClaw 的配对机制
- **容器隔离**：工具可在容器中运行
- **来源**: [Hermes 文档 - Security](https://hermes-agent.nousresearch.com/docs/user-guide/security)

两者安全模型相似，都采用配对 + allowlist + 命令审批模式。Hermes 额外支持容器隔离。

---

## 7. 安装部署

### OpenClaw

```bash
# 前置要求：Node 24（推荐）或 Node 22.16+
npm install -g openclaw@latest

# 引导式安装（推荐）
openclaw onboard --install-daemon

# 手动启动网关
openclaw gateway --port 18789 --verbose
```

- 支持 macOS、Linux、Windows (WSL2)
- Docker 安装可选
- Nix 声明式配置可选
- **来源**: [docs.openclaw.ai/start/getting-started](https://docs.openclaw.ai/start/getting-started)

### Hermes Agent

```bash
# 一键安装
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash

# 重载 shell
source ~/.bashrc  # 或 source ~/.zshrc

# 开始对话
hermes

# 完整设置向导
hermes setup

# 启动消息网关
hermes gateway setup
hermes gateway start
```

- 支持 Linux、macOS、WSL2、Android (Termux)
- Docker / SSH / Daytona / Modal 后端
- **来源**: [Hermes README](https://github.com/nousresearch/hermes-agent)

---

## 8. 迁移路径（OpenClaw → Hermes）

Hermes 提供了一键迁移工具：

```bash
# 交互式迁移（设置向导自动检测 ~/.openclaw）
hermes setup  # 自动检测并提示迁移

# 手动迁移
hermes claw migrate              # 完整迁移
hermes claw migrate --dry-run    # 预览迁移内容
hermes claw migrate --preset user-data   # 仅迁移用户数据（不含密钥）
hermes claw migrate --overwrite  # 覆盖已有配置
```

**迁移内容**：
- SOUL.md（人格文件）→ Hermes personality
- MEMORY.md / USER.md → Hermes 记忆系统
- Skills → `~/.hermes/skills/openclaw-imports/`
- 命令 allowlist → 审批模式
- 消息平台配置
- API keys（仅 allowlisted 的）
- TTS 资产、Workspace 指令

**来源**: [Hermes README - Migrating from OpenClaw](https://github.com/nousresearch/hermes-agent)

---

## 9. 社区活跃度

| 指标 | OpenClaw | Hermes Agent |
|------|----------|-------------|
| GitHub Stars | ~357,200 | ~83,500 |
| Stars 比例 | ~4.3:1 | 1 |
| Discord 社区 | discord.gg/clawd | discord.gg/NousResearch |
| 更新频率 | 活跃（每日更新） | 活跃（每日更新） |
| 生态项目 | awesome-openclaw-skills (46K stars) | HermesClaw (社区微信桥接) |
| 企业赞助 | OpenAI, GitHub, NVIDIA, Vercel 等 | 无公开赞助 |

**事实**：OpenClaw 社区规模约为 Hermes 的 4 倍以上，生态更成熟。

**来源**: GitHub API + README

---

## 10. 各自优缺点

### OpenClaw ✅ 优点
1. **平台覆盖最广**：25+ 消息平台，尤其中文生态（飞书、微信）无可替代
2. **社区最大**：357K stars，生态丰富（ClawHub 市场、awesome-skills）
3. **TypeScript 生态**：前端/全栈开发者更友好
4. **原生 App 支持**：macOS 菜单栏 App、iOS/Android 节点
5. **Live Canvas**：Agent 驱动的可视化工作区（A2UI）
6. **Voice Wake**：macOS/iOS 语音唤醒
7. **企业赞助**：有 OpenAI、NVIDIA 等背书

### OpenClaw ❌ 缺点
1. **无无服务器支持**：必须自建 VPS 运行
2. **记忆系统偏手动**：依赖文件和 AGENTS.md 指引，自动化程度低
3. **无自主技能创建**：技能需手动编写或从市场安装
4. **研究功能缺失**：无 RL 训练/轨迹生成能力

### Hermes Agent ✅ 优点
1. **自我进化**：内置学习循环，自动创建和改进技能
2. **记忆系统更智能**：FTS5 全文搜索 + LLM 摘要 + Honcho 用户画像
3. **无服务器支持**：Modal/Daytona 原生，$5 VPS 即可运行
4. **研究友好**：内置批量轨迹生成、Atropos RL 环境
5. **Python 生态**：AI/ML 研究者更友好
6. **一键迁移**：从 OpenClaw 迁移极其简便
7. **模型无锁定**：支持 200+ 模型（OpenRouter），一键切换

### Hermes Agent ❌ 缺点
1. **平台较少**：仅 6 个消息平台，缺少飞书、微信、Teams 等
2. **社区较小**：83K stars，生态尚在建设
3. **无原生 App**：无 macOS/iOS/Android 原生客户端
4. **无 Canvas/可视化**：缺少 Agent 驱动的可视化界面
5. **Python 依赖**：对非 Python 开发者门槛较高

---

## 11. 选型建议

| 使用场景 | 推荐 |
|----------|------|
| 需要飞书/微信接入 | **OpenClaw**（唯一选择） |
| 需要最多消息平台 | **OpenClaw** |
| 需要 AI 自主学习和进化 | **Hermes Agent** |
| 需要无服务器部署 | **Hermes Agent** |
| AI/ML 研究用途 | **Hermes Agent** |
| 全栈/前端开发者 | **OpenClaw** |
| Python/AI 研究者 | **Hermes Agent** |
| 企业级部署 | **OpenClaw**（生态更成熟） |
| 低成本个人使用 | **Hermes Agent**（$5 VPS + serverless） |

---

## 12. 来源汇总

- [OpenClaw GitHub](https://github.com/openclaw/openclaw)
- [OpenClaw 官方文档](https://docs.openclaw.ai)
- [OpenClaw 记忆文档](https://docs.openclaw.ai/concepts/memory)
- [Hermes Agent GitHub](https://github.com/nousresearch/hermes-agent)
- [Hermes Agent 官网](https://hermes-agent.nousresearch.com)
- [Hermes Agent DeepWiki](https://deepwiki.com/nousresearch-hermes-agent/hermes-agent)
- [VibeSparking 对比文章](https://www.vibesparking.com/en/blog/ai/openclaw/2026-04-09-openclaw-vs-hermes-agent-deep-comparison/)
- [DeployAgents 对比文章](https://www.deployagents.co/blog/openclaw-vs-hermes-agent-comparison)
- [HermesOS 对比文章](https://hermesos.cloud/blog/hermes-vs-openclaw)
- [PetronellaTech 对比文章](https://petronellatech.com/blog/openclaw-vs-hermes-agent-2026)
- [freeCodeCamp OpenClaw 教程](https://www.freecodecamp.org/news/how-to-build-and-secure-a-personal-ai-agent-with-openclaw)
- [OpenClaw 安全论文 (arXiv)](https://arxiv.org/html/2603.12644v1)
- [ClawHub 技能市场](https://clawhub.com)
- [AgentSkills 开放标准](https://agentskills.io)

---

*注：web_fetch 工具在本环境中被阻止（DNS 解析到私有 IP），部分对比文章（hermesos.cloud、deployagents.co 等）未能直接抓取，信息来源以 GitHub API、搜索摘要和可访问的页面为主。*
