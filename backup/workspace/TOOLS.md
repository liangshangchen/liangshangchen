# TOOLS.md - Local Notes

Skills define _how_ tools work. This file is for _your_ specifics — the stuff that's unique to your setup.

## What Goes Here

Things like:

- Camera names and locations
- SSH hosts and aliases
- Preferred voices for TTS
- Speaker/room names
- Device nicknames
- Anything environment-specific

## Examples

```markdown
### Cameras

- living-room → Main area, 180° wide angle
- front-door → Entrance, motion-triggered

### SSH

- home-server → 192.168.1.100, user: admin

### TTS

- Preferred voice: "Nova" (warm, slightly British)
- Default speaker: Kitchen HomePod
```

## Why Separate?

Skills are shared. Your setup is yours. Keeping them apart means you can update skills without losing your notes, and share skills without leaking your infrastructure.

---

Add whatever helps you do your job. This is your cheat sheet.

## 🔍 网络搜索配置

### 默认搜索引擎
- **日常搜索**：Bing CN（国内直接可用，速度快，中文支持好）
  - URL: `https://cn.bing.com/search?q={keyword}&ensearch=0`
  
### 备选搜索引擎
- **隐私搜索**：DuckDuckGo（不追踪，可能需要科学上网）
  - URL: `https://duckduckgo.com/html/?q={keyword}`
- **英文内容**：Bing INT
  - URL: `https://cn.bing.com/search?q={keyword}&ensearch=1`
- **知识计算**：WolframAlpha
  - URL: `https://www.wolframalpha.com/input?i={keyword}`

### 使用规则
1. 默认使用 **Bing CN**
2. 用户提到"隐私"、"不追踪"时切换到 **DuckDuckGo**
3. 搜索英文技术内容时可用 **Bing INT**
4. 需要计算/转换时用 **WolframAlpha**

---

## 🎤 语音配置（TTS）

### 默认音色
- **日常**：晓妮（陕西方言）- `zh-CN-shaanxi-XiaoniNeural`
- **正式**：云扬（新闻播报）- `zh-CN-YunyangNeural`

### 其他可用音色
- `zh-TW-YunJheNeural` - 云哲（台湾中文）
- `zh-CN-YunxiNeural` - 云希（年轻活泼）
- `zh-CN-YunjianNeural` - 云健（运动激情）
- `zh-CN-YunxiaNeural` - 云夏（卡通可爱）
- `zh-CN-XiaoxiaoNeural` - 晓晓（温暖女声）
- `zh-CN-XiaoyiNeural` - 晓伊（活泼女声）
- `zh-CN-liaoning-XiaobeiNeural` - 晓北（东北话）
- `zh-HK-WanLungNeural` - 云龙（香港中文）

### 使用规则
1. **默认**：晓妮（陕西方言）
2. **正式场合**：云扬（新闻播报）
3. 用户说"正式点"或"严肃点"时切换到云扬
4. **语音发送**：只在用户要求时才发语音，默认不发
