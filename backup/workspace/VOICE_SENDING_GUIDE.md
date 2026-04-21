# 语音发送操作流程

## 🎯 概述

使用 edge-tts 生成语音并发送到飞书的完整流程。

---

## 📋 完整流程

### 方式 1：三步流程（推荐）

```bash
# 步骤 1：生成 MP3 语音
cd ~/.openclaw/skills/edge-tts/scripts
node tts-converter.js "你的文本内容" \
  --voice zh-TW-YunJheNeural \
  --output ~/.openclaw/workspace/voice.mp3

# 步骤 2：转换为 Opus 格式
ffmpeg -y -i ~/.openclaw/workspace/voice.mp3 \
  -c:a libopus -b:a 32k \
  ~/.openclaw/workspace/voice.opus

# 步骤 3：发送到飞书
openclaw message send \
  --channel feishu \
  --target "ou_ad354496fffde21d16b656f608bafb70" \
  --media ~/.openclaw/workspace/voice.opus
```

---

### 方式 2：一行命令（快速）

```bash
# 生成、转换、发送一气呵成
cd ~/.openclaw/skills/edge-tts/scripts && \
node tts-converter.js "文本" --voice zh-TW-YunJheNeural --output ~/.openclaw/workspace/v.mp3 && \
ffmpeg -y -i ~/.openclaw/workspace/v.mp3 -c:a libopus -b:a 32k ~/.openclaw/workspace/v.opus && \
openclaw message send --channel feishu --target "ou_ad354496fffde21d16b656f608bafb70" --media ~/.openclaw/workspace/v.opus
```

---

## 🎤 可用音色

### 中文（日常）
| 音色 | 代码 | 特点 |
|------|------|------|
| 云哲（台湾）| `zh-TW-YunJheNeural` | **默认**，台湾口音 |
| 云希 | `zh-CN-YunxiNeural` | 年轻活泼 |
| 云扬 | `zh-CN-YunyangNeural` | 新闻播报，正式 |

### 中文（其他）
| 音色 | 代码 | 特点 |
|------|------|------|
| 云健 | `zh-CN-YunjianNeural` | 运动激情 |
| 云夏 | `zh-CN-YunxiaNeural` | 卡通可爱 |
| 云龙（香港）| `zh-HK-WanLungNeural` | 香港粤语 |

---

## 📝 使用示例

### 示例 1：日常语音（云哲）

```bash
cd ~/.openclaw/skills/edge-tts/scripts

node tts-converter.js \
  "嘿，NAN！今天天气不错，要不要出去走走？" \
  --voice zh-TW-YunJheNeural \
  --output ~/.openclaw/workspace/casual.mp3

ffmpeg -y -i ~/.openclaw/workspace/casual.mp3 \
  -c:a libopus -b:a 32k \
  ~/.openclaw/workspace/casual.opus

openclaw message send \
  --channel feishu \
  --target "ou_ad354496fffde21d16b656f608bafb70" \
  --media ~/.openclaw/workspace/casual.opus
```

---

### 示例 2：正式语音（云扬）

```bash
cd ~/.openclaw/skills/edge-tts/scripts

node tts-converter.js \
  "各位同事，今天的项目进度汇报如下..." \
  --voice zh-CN-YunyangNeural \
  --output ~/.openclaw/workspace/formal.mp3

ffmpeg -y -i ~/.openclaw/workspace/formal.mp3 \
  -c:a libopus -b:a 32k \
  ~/.openclaw/workspace/formal.opus

openclaw message send \
  --channel feishu \
  --target "ou_ad354496fffde21d16b656f608bafb70" \
  --media ~/.openclaw/workspace/formal.opus
```

---

## 🔧 参数说明

### tts-converter.js 参数

| 参数 | 说明 | 示例 |
|------|------|------|
| 第一个参数 | 要转换的文本 | "你好世界" |
| `--voice` | 音色代码 | `zh-TW-YunJheNeural` |
| `--output` | 输出文件路径 | `~/output.mp3` |
| `--rate` | 语速（可选）| `+0%` / `-10%` |

### ffmpeg 参数

| 参数 | 说明 | 推荐值 |
|------|------|--------|
| `-i` | 输入文件 | `voice.mp3` |
| `-c:a` | 音频编码 | `libopus` |
| `-b:a` | 比特率 | `32k` |
| 输出文件 | 输出文件 | `voice.opus` |

### openclaw message send 参数

| 参数 | 说明 | 示例 |
|------|------|------|
| `--channel` | 渠道 | `feishu` |
| `--target` | 接收者 ID | `ou_xxx...` |
| `--media` | 媒体文件路径 | `voice.opus` |

---

## 📂 文件路径

### 推荐路径

```bash
# 工作区（推荐）
~/.openclaw/workspace/voice.mp3
~/.openclaw/workspace/voice.opus

# 临时路径
/tmp/voice.mp3
/tmp/voice.opus
```

### 为什么用 workspace？

- ✅ `openclaw message send` 允许访问
- ✅ 方便管理
- ✅ 不会被系统清理

---

## ⚡ 快速参考

### 最常用命令

```bash
# 生成语音（日常）
cd ~/.openclaw/skills/edge-tts/scripts
node tts-converter.js "文本" --voice zh-TW-YunJheNeural --output ~/.openclaw/workspace/v.mp3
ffmpeg -y -i ~/.openclaw/workspace/v.mp3 -c:a libopus -b:a 32k ~/.openclaw/workspace/v.opus
openclaw message send --channel feishu --target "ou_ad354496fffde21d16b656f608bafb70" --media ~/.openclaw/workspace/v.opus
```

### 生成语音（正式）

```bash
# 把 --voice 改为云扬即可
--voice zh-CN-YunyangNeural
```

---

## 🎯 发送给自己

```bash
# 你的飞书 ID
--target "ou_ad354496fffde21d16b656f608bafb70"
```

### 发送给其他人

```bash
# 替换为对方的飞书 ID
--target "ou_xxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
```

---

## 💡 匸见问题

### Q: 文件太大怎么办？

```bash
# 降低比特率
ffmpeg -i voice.mp3 -c:a libopus -b:a 24k voice.opus
```

### Q: 想要更快语速？

```bash
# 加速 20%
node tts-converter.js "文本" --voice zh-TW-YunJheNeural --rate +20% --output v.mp3
```

### Q: 想要更慢语速？

```bash
# 减速 20%
node tts-converter.js "文本" --voice zh-TW-YunJheNeural --rate -20% --output v.mp3
```

---

## 📋 总结

### 三步流程

1. **生成 MP3** → `node tts-converter.js`
2. **转换 Opus** → `ffmpeg`
3. **发送飞书** → `openclaw message send`

### 默认音色

- **日常**：`zh-TW-YunJheNeural`（云哲，台湾）
- **正式**：`zh-CN-YunyangNeural`（云扬，新闻）

### 默认接收者

- 你的飞书 ID：`ou_ad354496fffde21d16b656f608bafb70`

---

*最后更新：2026-03-10*
