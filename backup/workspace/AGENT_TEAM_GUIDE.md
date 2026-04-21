# Agent 团队配置完成

## 🎉 已创建的 Agent 团队

| Agent | 角色 | 职责 | Prompt 文件 |
|-------|------|------|------------|
| **main** 🦀 | 全能主 Agent | 日常对话、协调 | IDENTITY.md |
| **product** 📋 | 产品经理 | 产品规划、需求管理 | IDENTITY.md |
| **operations** 📊 | 运营 | 用户增长、数据分析 | IDENTITY.md |
| **tester** 🧪 | 测试工程师 | 质量保证、测试设计 | IDENTITY.md |
| **developer** 💻 | 开发工程师 | 代码开发、技术实现 | IDENTITY.md |
| **frontend** 🎨 | 前端开发 | UI 开发、性能优化 | IDENTITY.md |
| **backend** ⚙️ | 后端开发 | API 设计、系统架构 | IDENTITY.md |
| **architect** 🏗️ | 架构师 | 架构设计、技术选型 | IDENTITY.md |
| **project** 📅 | 项目经理 | 项目管理、团队协调 | IDENTITY.md |

---

## 🚀 使用方式

### 方式 1：路由绑定（自动切换）

```bash
# 绑定关键词到特定 agent
openclaw agents bind developer --keyword "写代码|编程|debug|bug修复"
openclaw agents bind tester --keyword "测试|test|QA"
openclaw agents bind architect --keyword "架构|设计|技术选型"
```

### 方式 2：我主动调用（子任务）

```
你: "帮我写个爬虫"

我: "好的，让我调用 developer agent..."
→ spawn developer agent
→ developer 完成任务
→ 我返回结果给你
```

### 方式 3：直接对话

```bash
# 切换到特定 agent
openclaw use developer

# 或通过渠道绑定
openclaw agents bind developer --channel slack:dev-team
```

---

## 📋 Prompt 说明

每个 agent 都有专门的 IDENTITY.md，包含：

### 1. 核心职责
- 明确的角色定位
- 具体的工作职责
- 专业的思考框架

### 2. 工作方法
- 标准的工作流程
- 常用的模板
- 最佳实践

### 3. 协作关系
- 与其他角色的协作方式
- 输入输出标准
- 沟通机制

### 4. 工具使用
- 常用工具列表
- 使用场景说明

---

## 🎯 如何定制

### 修改 Prompt

```bash
# 编辑特定 agent 的 prompt
vim ~/.openclaw/agents/developer/agent/IDENTITY.md

# 修改后立即生效（无需重启）
```

### 添加新 Agent

```bash
# 创建新 agent
openclaw agents add new-role

# 编写 prompt
vim ~/.openclaw/agents/new-role/agent/IDENTITY.md
```

---

## 💡 使用示例

### 场景 1：开发任务

```
你: "帮我写个登录功能"

我: "好的，让我调用 developer agent 来处理..."
→ developer agent 开始工作
→ 输出代码和文档
→ 我整理后发给你
```

### 场景 2：架构设计

```
你: "设计一个高并发系统"

我: "架构设计需要 architect agent..."
→ architect agent 分析需求
→ 输出架构方案
→ 我补充说明后发给你
```

### 场景 3：项目规划

```
你: "帮我规划下个项目"

我: "项目规划找 project agent..."
→ project agent 制定计划
→ 输出项目计划文档
→ 我确认后发给你
```

---

## 📊 Prompt 模板结构

每个 IDENTITY.md 包含：

```markdown
# [角色名称] Agent

## 🎯 身份
角色定位和核心价值

## 📋 核心职责
- 职责 1
- 职责 2
- ...

## 🛠️ 工作方法
具体的工作流程和方法

## 💡 思考框架
专业的思维模型

## 🎯 输出标准
交付物的质量标准

## 🔗 协作关系
与其他角色的协作方式

## 📝 常用工具
推荐的工具列表
```

---

## 🔄 后续优化

你可以：

1. **修改 Prompt** - 根据实际使用调整
2. **添加新 Agent** - 扩展团队
3. **优化路由** - 调整触发条件
4. **共享配置** - 团队统一使用

---

*创建时间：2026-03-10*
*状态：✅ 已完成*
