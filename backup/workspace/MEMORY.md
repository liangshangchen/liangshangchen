# MEMORY.md - 长期记忆

## 项目：douyinchat（抖音私信自动回复）

### 核心信息
- **仓库**：`/Users/liangshangchen/Documents/PyCharm_Work/douyinchat`
- **功能**：抖音私信未读消息查看 + 自动回复
- **技术栈**：Python + Appium 真机自动化

### 店铺业务背景
- 主体：山东威海海纳百川制药集团直营店铺
- 特点：工厂直供，非机构、非代理商
- 合作模式：分销、现结、一件代发、OEM、ODM
- 目标：建立信任、推进合作沟通

### 回复 Skill 配置
- **配置文件**：`config/reply_skill_prompt.md`
- **自动化提供的字段**：`peer_name`（对方昵称）、`latest_inbound_text`（对方最新消息）、`visible_context`（最近几条可见消息）
- **模型输出 JSON**：
  ```json
  {
    "should_reply": true,
    "reply_text": "string",
    "tone": "friendly_business",
    "reason": "string",
    "risk_flags": []
  }
  ```
- **只消费**：`should_reply` 和 `reply_text`，其余写日志
- **调试开关**：`debug_fixed_template=true` 走固定文案兜底，不调模型

### 目标人物
- **梁上尘**：抖音达人，有回复记录，target 配置在 `data/targets/liangshangchen.json`

---

## 项目：douyindianshang_jxlm（抖音小店精选联盟达人采集）

### 核心信息
- **仓库**：`/Users/liangshangchen/Documents/PyCharm_Work/douyindianshang_jxlm`
- **子目录**：`douyin-buyin-daren-scanner`
- **技术栈**：Python 3.11 + Playwright + Chrome CDP（非 Appium）
- **Conda 环境**：`app`（`~/miniconda3/envs/app/bin/python`）
- **Skill 文件**：`~/.openclaw/workspace/skills/douyin-buyin-daren-scanner/SKILL.md`

### Chrome CDP 配置
- **专用 profile**：`~/buyin-cdp-profile`（不混用普通 Chrome）
- **启动命令**：
  ```bash
  /Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome \
    --remote-debugging-port=9222 \
    --user-data-dir="$HOME/buyin-cdp-profile"
  ```
- **CDP 检查**：`curl http://127.0.0.1:9222/json/version`
- **注意**：Chrome 136+ 必须用非默认 user-data-dir，复制默认 profile 的 cookie 不可靠（加密密钥不同），需在专用 Chrome 里手动登录

### CLI 入口
- 源码：`~/miniconda3/envs/app/bin/python src/cli_main.py`
- 打包二进制：`dist/buyin-cli-macos-x64-onedir/buyin-cli/buyin-cli`

### 工作流
1. `smoke` → 检查 CDP + 登录态
2. `scan --fresh-start --max-pages N --max-rows-per-page N` → 扫描达人
3. `invite --run-id <id> --max-invites N --force` → dry-run 邀约
4. `invite --run-id <id> --max-invites 1 --force --send-live` → 真实发送
5. `export-xlsx --run-id <id>` → 导出报表

### 筛选配置
- **配置文件**：`config/filters.yaml`
- 当前：直播达人 / 滋补保健 / 未沟通过
- **选择器**：`config/selectors.yaml`（页面改版可能需更新）

---

## 项目：douyinxiaodian（抖店达人采集 - 旧版 Appium）

### 核心信息
- **仓库**：`/Users/liangshangchen/Documents/PyCharm_Work/douyinxiaodian`
- **技术栈**：Python + Appium + CLI（`douyinxiaodian` 命令）
- **Conda 环境**：`app`（Python 3.11.15）
- **Skill 文件**：`~/.openclaw/workspace/skills/douyin-scan/SKILL.md`

### 设备配置
- **旧手机**：`2734a6c5`（local_android）
- **新手机**：`34209ecc`（local_android_2，小米10 M2007J1SC）
- **Appium 端口**：4723

### 模式
| 模式 | 触发词 | 配置文件 | 说明 |
|------|--------|----------|------|
| scan（浅层） | 扫达人、scan达人 | runtime_scan.yaml | 列表页基础数据 |
| scan deep（深度） | 深扫、scan deep | runtime_scan_deep.yaml | 进详情页，采微信号+profile |
| capture（按名） | 采集达人、按名抓取 | — | 搜达人昵称采联系方式 |

### 深度扫描数据
- 微信号：`wechat_text`（scan 模式只采微信，不采手机号）
- 详情页 profile：4 个 tab（概览/直播/视频/带货），含详细 metrics
- 数据看 report JSON，不看日志（日志不打印字段级提取值）

### 已知问题
- 深度扫描每个达人约 2-3 分钟，10 个需 15-20 分钟
- 主 agent 无法在 turn 内多次发消息，需用 sub-agent 实现定时进度汇报
- exec timeout 限制，长任务需 background + sub-agent 监控

---

## 运营规则（2026-04-04 约定）

- 跑蝉妈妈/精选联盟/抖音私信时，如果界面没打开，自动打开 Chrome CDP 并导航到对应页面
- 自动检测登录态，只有确实未登录时才告诉用户
- 不让用户手动打开浏览器或登录
- 跑完必须发 XLSX 原文件 + 业务分析，不能只给路径
- 汇报按业务口径，不讲 CDP/端口/selector 等技术细节（除非用户追问或跑不起来）

## 教训（2026-04-17 迁移到新 MacBook）

- **先排查再操作**：不要上来就叫用户重装，先定位根因（这次是 nvm 没初始化到 .zshrc 导致 PATH 找不到全局命令）
- **新机器 .zshrc 需要配的**：brew shellenv、nvm/node PATH、Java PATH
- **p10k 没有 COLOR_SCHEME 变量**：颜色通过 `p10k configure` 向导或直接改各段样式
- **nvm 可能不是通过 brew 装的**：OpenClaw 安装脚本自带 nvm，路径在 `~/.nvm/versions/node/`，不一定有 `/opt/homebrew/opt/nvm/nvm.sh`

---

## 工具环境

### scrcpy（手机投屏）
- 安装方式：GitHub release 预编译版（brew 在 macOS 13 上编译失败）
- 版本：3.3.4（macOS x86_64）
- 路径：`/usr/local/bin/scrcpy-bin/scrcpy`
- 用法：`scrcpy -s <udid> --max-size=800`

### 微信 ClawBot
- 插件已安装：`~/.openclaw/extensions/openclaw-weixin/`
- 登录命令：`openclaw channels login --channel openclaw-weixin`
- 状态：已连接成功，但消息回复待验证

---

## 日期：2026-03-26

### 今日事件
- douyinxiaodian 新设备接入（小米10），smoke/scan/scan deep 均验证通过
- douyinxiaodian skill 更新：新增 scan deep 模式、sub-agent 定时汇报、CLI 入口切换
- douyinchat 项目拉取，新增 reply flow 完整链路（干跑+实发）
- scrcpy 投屏安装完成（GitHub release 预编译版）
- 微信 ClawBot 插件安装并扫码绑定成功
