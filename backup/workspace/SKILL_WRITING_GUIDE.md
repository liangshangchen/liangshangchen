# Skill 编写通用规则

基于 OpenAI Skills 和 Claude Skills 官方仓库总结的最佳实践。

---

## 📁 目录结构

```
skill-name/
├── SKILL.md              # 必需：技能主文件
├── README.md             # 可选：用户文档
├── LICENSE.txt           # 可选：许可证
├── scripts/              # 可选：可执行脚本
│   ├── setup.sh
│   └── helper.py
├── references/           # 可选：详细文档
│   ├── api-guide.md
│   └── examples.md
└── assets/               # 可选：资源文件
    ├── templates/
    └── images/
```

---

## 📝 SKILL.md 结构

### 1. YAML Frontmatter（必需）

```yaml
---
name: skill-name                    # 技能标识符（小写，连字符）
description: |                      # 何时触发、做什么（关键！）
  清晰描述技能的功能和触发条件。
  这是主要的触发机制，必须包含：
  1. 技能做什么
  2. 何时使用
  3. 具体场景
license: Apache-2.0                 # 可选：许可证
---
```

#### description 写法要点

**好的 description 示例**：
```yaml
description: |
  Use this skill whenever the user wants to do anything with PDF files. 
  This includes reading or extracting text/tables from PDFs, combining or 
  merging multiple PDFs into one, splitting PDFs apart, rotating pages, 
  adding watermarks, creating new PDFs, filling PDF forms, encrypting/decrypting 
  PDFs, extracting images, and OCR on scanned PDFs to make them searchable. 
  If the user mentions a .pdf file or asks to produce one, use this skill.
```

**关键要素**：
- ✅ 明确的触发条件
- ✅ 具体的使用场景
- ✅ 涵盖所有功能
- ✅ 稍微"pushy"（防止 under-trigger）

---

### 2. 正文结构

```markdown
# Skill Name

简短的概述，说明技能的用途和价值。

## Overview / 概述

详细说明技能的功能、适用场景、核心能力。

## Quick Start / 快速开始

提供最简单的使用示例，让用户快速上手。

```python
# 示例代码
from library import Tool
tool = Tool()
result = tool.do_something()
```

## Core Features / 核心功能

### Feature 1

详细说明功能1的使用方法和示例。

### Feature 2

详细说明功能2的使用方法和示例。

## Common Tasks / 常见任务

列出最常见的使用场景和解决方案。

| Task | Best Tool | Command |
|------|-----------|---------|
| Task 1 | tool-1 | `command` |
| Task 2 | tool-2 | `command` |

## Examples / 示例

**Example 1:**
Input: ...
Output: ...

**Example 2:**
Input: ...
Output: ...

## Guidelines / 指南

- 指南1
- 指南2
- 指南3

## References / 参考

- [参考文档1](./references/doc1.md)
- [参考文档2](./references/doc2.md)
```

---

## ✍️ 写作风格

### 1. 使用祈使语气

**好**：
```markdown
Create a new file and add the following content.
Run the script to generate the output.
```

**不好**：
```markdown
You should create a new file.
The script needs to be run.
```

### 2. 解释"为什么"

**好**：
```markdown
Use async/await for I/O operations because it improves performance and 
prevents blocking the main thread.
```

**不好**：
```markdown
MUST use async/await for I/O operations.
ALWAYS use async/await.
```

### 3. 提供具体示例

**好**：
```markdown
**Example:**
```python
from pypdf import PdfReader

reader = PdfReader("document.pdf")
text = reader.pages[0].extract_text()
```
```

**不好**：
```markdown
Use the library to extract text from PDFs.
```

### 4. 保持简洁

- ✅ SKILL.md < 500 行（理想）
- ✅ 超过 300 行的文件放到 references/
- ✅ 使用表格快速展示信息

---

## 🎯 触发机制

### description 是关键

Claude/LLM 根据 description 决定是否使用技能：

1. **明确触发条件**
   ```yaml
   description: Use this skill when the user mentions "dashboard" or "data visualization"
   ```

2. **涵盖同义词**
   ```yaml
   description: |
     Use this skill when the user mentions:
     - PDF files
     - .pdf extension
     - document extraction
     - text from scanned documents
   ```

3. **防止 under-trigger**
   ```yaml
   description: |
     Make sure to use this skill whenever the user mentions dashboards, 
     data visualization, internal metrics, or wants to display any kind 
     of company data, even if they don't explicitly ask for a 'dashboard.'
   ```

---

## 📚 渐进式加载

Skills 使用三级加载系统：

| 级别 | 内容 | 大小限制 |
|------|------|----------|
| **1. 元数据** | name + description | ~100 words |
| **2. SKILL.md 正文** | 主要指令 | < 500 lines |
| **3. 打包资源** | scripts, references | 无限制 |

**关键模式**：
- 保持 SKILL.md 简洁
- 清晰引用其他文件
- 大文件（>300行）包含目录

---

## 🔧 组织多领域技能

当技能支持多个框架/平台时：

```
cloud-deploy/
├── SKILL.md (workflow + selection)
└── references/
    ├── aws.md
    ├── gcp.md
    └── azure.md
```

Claude 只读取相关的参考文件。

---

## 🛡️ 安全原则

### Principle of Lack of Surprise

技能**不得包含**：
- ❌ 恶意软件
- ❌ 利用代码
- ❌ 可能危及系统安全的内容
- ❌ 误导性内容
- ❌ 未经授权访问的工具

**允许**：
- ✅ "角色扮演"类技能
- ✅ 合法的自动化工具
- ✅ 教育性示例

---

## 📋 质量检查清单

### 必需项

- [ ] YAML frontmatter 包含 name 和 description
- [ ] description 清晰描述触发条件
- [ ] 提供快速开始示例
- [ ] 代码示例可运行
- [ ] 文件结构清晰

### 推荐项

- [ ] SKILL.md < 500 行
- [ ] 包含常见任务表格
- [ ] 提供多个具体示例
- [ ] 解释"为什么"而不是强制规则
- [ ] 大文件放到 references/

### 可选项

- [ ] 包含 scripts/ 目录
- [ ] 包含 assets/ 目录
- [ ] 提供 LICENSE.txt
- [ ] 提供多语言支持

---

## 🎨 输出格式定义

### 使用模板

```markdown
## Report Structure

ALWAYS use this exact template:

# [Title]

## Executive Summary
[2-3 sentence summary]

## Key Findings
- Finding 1
- Finding 2

## Recommendations
1. Recommendation 1
2. Recommendation 2
```

---

## 📊 示例模板

### 最小化技能

```
my-skill/
└── SKILL.md
```

```yaml
---
name: my-skill
description: A clear description of what this skill does and when to use it.
---

# My Skill

Brief overview of the skill.

## Quick Start

```python
# Simple example
```

## How to Use

Instructions here.
```

### 完整技能

```
document-processor/
├── SKILL.md
├── README.md
├── LICENSE.txt
├── scripts/
│   ├── convert.py
│   └── validate.py
├── references/
│   ├── api.md
│   ├── formats.md
│   └── troubleshooting.md
└── assets/
    ├── templates/
    │   └── report.html
    └── examples/
        └── sample.pdf
```

---

## 🚀 最佳实践

### 1. 从简单开始

1. 写一个最小化的 SKILL.md
2. 测试触发是否正确
3. 逐步添加详细内容

### 2. 测试触发

创建测试用例：
```json
[
  {"query": "read this PDF", "should_trigger": true},
  {"query": "create a new file", "should_trigger": false}
]
```

### 3. 迭代改进

- 收集用户反馈
- 优化 description
- 添加更多示例
- 扩展参考文档

---

## 📖 参考资源

### OpenAI Skills
- 仓库：`~/.openclaw/workspace/openai-skills/`
- 特点：30+ 生产可用技能

### Claude Skills
- 仓库：`~/.openclaw/workspace/claude-official-skills/`
- 特点：Anthropic 官方维护

### 官方文档
- Agent Skills 规范：http://agentskills.io
- Claude Skills 文档：https://support.claude.com/en/articles/12512176-what-are-skills

---

## 总结

一个好的 skill 应该：

1. **清晰的触发条件** - description 是关键
2. **简洁的结构** - SKILL.md < 500 行
3. **具体的示例** - 代码可运行
4. **解释原因** - 而不是强制规则
5. **渐进式加载** - 分层组织内容
6. **安全可靠** - 无恶意内容

---

*基于 OpenAI Skills 和 Claude Skills 官方仓库总结*
