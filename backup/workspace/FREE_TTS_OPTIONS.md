# 免费音色选项大全

## 🎤 已安装（免费）

### 1. edge-tts（推荐）⭐
- **来源**：Microsoft Edge
- **价格**：完全免费
- **音色数量**：100+ 种
- **中文音色**：
  - 云哲（台湾）：zh-TW-YunJheNeural
  - 云扬（新闻）：zh-CN-YunyangNeural
  - 云希（年轻）：zh-CN-YunxiNeural
  - 云健（运动）：zh-CN-YunjianNeural
  - 云夏（可爱）：zh-CN-YunxiaNeural
  - 云龙（香港）：zh-HK-WanLungNeural
- **优点**：质量高、免费、无需 API
- **缺点**：需要网络

### 2. mac-tts（已安装）
- **来源**：macOS 系统自带
- **价格**：完全免费
- **音色数量**：系统内置
- **优点**：离线、稳定
- **缺点**：音色质量一般

---

## 🆓 在线免费 TTS

### 3. Google Translate TTS
- **价格**：免费
- **语言**：支持多种语言
- **方式**：通过 URL 直接调用
- **示例**：
  ```
  https://translate.google.com/translate_tts?ie=UTF-8&tl=zh-CN&client=tw-ob&q=你好
  ```

### 4. 百度翻译 TTS
- **价格**：免费（有限额）
- **语言**：中文、英文
- **方式**：通过 URL 调用
- **需要**：网络

### 5. 有道词典 TTS
- **价格**：免费
- **语言**：中文、英文
- **方式**：通过 URL 调用

---

## 🏢 云服务免费额度

### 6. 讯飞 TTS
- **免费额度**：每日 500 次
- **音色**：多种中文音色
- **质量**：高
- **需要**：注册账号、API Key

### 7. 阿里云 TTS
- **免费额度**：每月 100 万字符
- **音色**：多种音色
- **质量**：高
- **需要**：阿里云账号、Access Key

### 8. 腾讯云 TTS
- **免费额度**：每月 100 万字符
- **音色**：多种音色
- **质量**：高
- **需要**：腾讯云账号、Secret Key

### 9. 百度智能云 TTS
- **免费额度**：每日 50000 次
- **音色**：多种音色
- **质量**：高
- **需要**：百度云账号、API Key

---

## 🤖 开源 AI TTS（本地运行）

### 10. ChatTTS（推荐）⭐
- **价格**：完全免费
- **方式**：本地运行
- **特点**：AI 生成，自然度高
- **需要**：GPU、模型下载
- **GitHub**：https://github.com/2noise/ChatTTS

### 11. Fish Speech（推荐）⭐
- **价格**：完全免费
- **方式**：本地运行
- **特点**：高质量中文语音
- **需要**：GPU、模型下载
- **GitHub**：https://github.com/fishaudio/fish-speech

### 12. Bark
- **价格**：完全免费
- **方式**：本地运行
- **特点**：Suno AI 开源，支持多种语言
- **需要**：GPU、模型下载
- **GitHub**：https://github.com/suno-ai/bark

### 13. Tortoise TTS
- **价格**：完全免费
- **方式**：本地运行
- **特点**：高质量、支持声音克隆
- **需要**：GPU、模型下载
- **GitHub**：https://github.com/neonbjb/tortoise-tts

### 14. Coqui TTS
- **价格**：完全免费
- **方式**：本地运行
- **特点**：多种模型、支持声音克隆
- **需要**：GPU、模型下载
- **GitHub**：https://github.com/coqui-ai/TTS

### 15. GPT-SoVITS（推荐）⭐
- **价格**：完全免费
- **方式**：本地运行
- **特点**：少样本声音克隆、中文优化
- **需要**：GPU、模型下载
- **GitHub**：https://github.com/RVC-Boss/GPT-SoVITS

### 16. VITS
- **价格**：完全免费
- **方式**：本地运行
- **特点**：高质量、支持声音克隆
- **需要**：GPU、模型下载
- **GitHub**：https://github.com/jaywalnut310/vits

---

## 🎮 其他免费选项

### 17. ResponsiveVoice.js
- **价格**：免费（非商业）
- **方式**：浏览器端
- **音色**：多种音色
- **网站**：https://responsivevoice.org/

### 18. SpeechSynthesis（浏览器原生）
- **价格**：完全免费
- **方式**：浏览器 API
- **音色**：系统音色
- **示例**：
  ```javascript
  const utterance = new SpeechSynthesisUtterance('你好');
  speechSynthesis.speak(utterance);
  ```

### 19. pyttsx3（Python）
- **价格**：完全免费
- **方式**：离线运行
- **音色**：系统音色
- **安装**：`pip install pyttsx3`

---

## 📊 对比总结

| 选项 | 免费 | 离线 | 质量 | 中文 | 推荐度 |
|------|------|------|------|------|--------|
| edge-tts | ✅ | ❌ | ⭐⭐⭐⭐⭐ | ✅ | ⭐⭐⭐⭐⭐ |
| mac-tts | ✅ | ✅ | ⭐⭐⭐ | ✅ | ⭐⭐⭐ |
| ChatTTS | ✅ | ✅ | ⭐⭐⭐⭐⭐ | ✅ | ⭐⭐⭐⭐⭐ |
| Fish Speech | ✅ | ✅ | ⭐⭐⭐⭐⭐ | ✅ | ⭐⭐⭐⭐⭐ |
| GPT-SoVITS | ✅ | ✅ | ⭐⭐⭐⭐⭐ | ✅ | ⭐⭐⭐⭐⭐ |
| 讯飞 TTS | 限额 | ❌ | ⭐⭐⭐⭐ | ✅ | ⭐⭐⭐⭐ |
| 阿里云 TTS | 限额 | ❌ | ⭐⭐⭐⭐ | ✅ | ⭐⭐⭐⭐ |
| Bark | ✅ | ✅ | ⭐⭐⭐⭐ | ⚠️ | ⭐⭐⭐ |

---

## 🚀 推荐方案

### 方案 1：继续用 edge-tts（最简单）
- ✅ 已经安装
- ✅ 质量高
- ✅ 免费
- ✅ 音色多

### 方案 2：本地 AI TTS（最高质量）
- **ChatTTS** 或 **Fish Speech**
- ✅ 完全离线
- ✅ 质量最高
- ✅ 支持声音克隆
- ❌ 需要 GPU
- ❌ 需要下载模型

### 方案 3：云服务免费额度（适合生产）
- **讯飞 TTS** 或 **阿里云 TTS**
- ✅ 稳定可靠
- ✅ 质量高
- ❌ 有配额限制
- ❌ 需要注册账号

---

## 💡 我的建议

**对于你的使用场景（个人使用）**：

1. **继续用 edge-tts** - 已经够用了
2. **如果想更高品质** - 试试 ChatTTS 或 Fish Speech（需要 GPU）
3. **如果想要特定音色** - GPT-SoVITS 可以克隆声音

---

*最后更新：2026-03-11*
