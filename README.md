# DeepThink

多厂商 AI Agent 框架 — 15 个模型厂商、工具系统、上下文管理、多模态、本地模型。

---

## 快速开始

```bash
npm install -g .      # 全局安装
deepthink setup       # 首次配置向导
deepthink tui         # 启动 TUI
deepthink "用 TypeScript 写一个快速排序"   # 单次对话
```

## 命令

| 命令                       | 说明                         |
| -------------------------- | ---------------------------- |
| `deepthink`                | 单次对话                     |
| `deepthink tui`            | 全屏终端界面（推荐）         |
| `deepthink tui --guardian` | TUI + 守护进程，崩溃自动重启 |
| `deepthink tui --continue` | 继续上一次会话               |
| `deepthink serve`          | HTTP API 服务器              |

## 支持的厂商

设环境变量即可，多个 Key 同时设了自动构建降级链：

```
ANTHROPIC_API_KEY   Claude       OPENAI_API_KEY    GPT
DEEPSEEK_API_KEY    DeepSeek     GEMINI_API_KEY    Gemini
DASHSCOPE_API_KEY   Qwen 阿里     ZHIPU_API_KEY     智谱 GLM
MINIMAX_API_KEY     MiniMax      MIMO_API_KEY      小米 MiMo
GROQ_API_KEY        Groq         XAI_API_KEY       Grok
MISTRAL_API_KEY     Mistral      MOONSHOT_API_KEY  Kimi
OPENROUTER_API_KEY  200+ 模型
```

## 飞书渠道

支持通过飞书机器人接收消息并回复。配置方式：直接在tui中告诉模型你的id及secret 或者在 `.agent/config.json` 中添加：

```json
{
  "channel": {
    "type": "feishu",
    "appId": "cli_xxxxxxxxxxxxxxxx",
    "appSecret": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
  }
}
```

- `appId`: 飞书应用 ID（从飞书开发者后台获取）
- `appSecret`: 飞书应用密钥（从飞书开发者后台获取）

## TUI 快捷键

`Ctrl+C` 退出 · `Ctrl+P` 切厂商 · `Ctrl+L` 清屏 · `/` 命令面板

## 首次运行自动创建

`~/.agent/` 全局配置 · `.agent/` 项目配置 · sessions 对话记录 · tools 热插拔目录
