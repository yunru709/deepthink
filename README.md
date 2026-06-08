# DeepThink

下面这些东西大体上ai写的，凑活看吧，我休息几天，累的快圆寂了

叫这个名字是因为老多代码是deepseek干的，而且我手里只有deepseek的api，确认过能跑

保留一切权利，禁止商用。(暂时)

我没给他加搜索工具，你可以让他自己写一个装上，或者使用：{
  "mcpServers": {
    "chrome-devtools": {
      "command": "npx",
      "args": ["-y", "chrome-devtools-mcp@latest"]
    }
  }
}
发给他它自己会安的。
还有：https://github.com/CursorTouch/Windows-MCP  网址给他就行

---

## 设计理念 · Design Philosophy

### 1. 大模型是一个自回归函数

LLM 的输出和输入之间存在严格的因果链 — 它本质上是一个极其复杂的自回归函数，next token 完全由前文决定。这意味着：**垃圾进，垃圾出；优质上下文进，才有可能优质结果出。** 没有什么魔法，只有输入质量决定输出质量。

### 2. 上下文污染是核心敌人

每一条工具返回、每一段系统提示、每一个 skill 定义 — 都在占用宝贵的上下文窗口。混乱的、冗余的、不相关的信息会"污染"模型推理空间，拉低输出质量。所以 DeepThink 的核心思路不是"更好看的缓存命中数据"，而是“更加强大的agent助手”

### 3. 缓存命中率 vs 完成任务能力

缓存友好的做法是把东西固定住不动，但我们不行 — 因为我们要动态筛选上下文。**当前版本只在工具方面做了调整，一般来说上下文缓存命中率经常在 90%~99%。**

但诚实地说：下个版本会加入知识库（RAG）和 Precise 模式的深度优化，届时动态内容增多，**在精确模式下缓存命中率会掉**。

这是有意为之的取舍：**牺牲缓存命中率，提高模型完成任务的能力。** 缓存是经济手段，但完成任务才是agent的目的。

---



## 安装 · Installation

### 前置条件 · Prerequisites

- **Node.js** >= 18（推荐 20+）· _Node.js >= 18 (20+ recommended)_

### 安装步骤 · Steps

**1. 全局安装：**

```powershell
cd 本文件夹
npm install -g .
```

安装完成后 `deepthink` 命令全局可用，原文件夹可以删除。

_After installation the `deepthink` command is available globally. The original folder can be deleted._

**2. 设置 API Key：**

_Option 1 — Environment variable (temporary, current terminal only):_

```powershell
$env:DEEPSEEK_API_KEY = "sk-your-key-here"
```

方式二 — `.env` 文件（持久化，推荐）：

_Option 2 — `.env` file (persistent, recommended):_

```powershell
deepthink setup
```

或手动创建 `~/.agent/.env`：

_Or manually create `~/.agent/.env`:_

```
DEEPSEEK_API_KEY=sk-your-key-here
ANTHROPIC_API_KEY=sk-ant-your-key-here
OPENAI_API_KEY=sk-your-key-here
```

**3. 验证安装：**

```powershell
deepthink --help
```

应输出命令列表。

_Should print the command list._

### 卸载 · Uninstall

```powershell
npm uninstall -g deepthink
```

---

## 快速开始 · Quick Start

```powershell
# 配置向导（首次使用）· Setup wizard (first time)
deepthink setup

# 启动 TUI · Start TUI
deepthink --tui
```

如需自动重启支持（飞书渠道配置等场景），加 `--guardian`：

_Use `--guardian` for automatic restart support (e.g. after channel config changes):_

```powershell
deepthink --tui --guardian
```

---

## 支持的 Provider · Supported Providers

通过环境变量或 `.agent/.env` 文件设置 API Key：

_Set API keys via environment variables or `.agent/.env`:_

| Provider        | 环境变量 · Env Variable |
| --------------- | ----------------------- |
| Anthropic       | `ANTHROPIC_API_KEY`     |
| OpenAI          | `OPENAI_API_KEY`        |
| DeepSeek        | `DEEPSEEK_API_KEY`      |
| Google Gemini   | `GEMINI_API_KEY`        |
| Groq            | `GROQ_API_KEY`          |
| xAI             | `XAI_API_KEY`           |
| Mistral         | `MISTRAL_API_KEY`       |
| OpenRouter      | `OPENROUTER_API_KEY`    |
| Moonshot / Kimi | `MOONSHOT_API_KEY`      |

同时支持本地模型（llama.cpp、Ollama、vLLM、LM Studio）。

_Local models (llama.cpp, Ollama, vLLM, LM Studio) are also supported._

---

## 核心功能 · 

功能：
其他agent都有的功能，
引导(模仿小龙虾的)，
提供模式TODO/Plan/Spec，
具有较高的自由度它能修改自己不少的配置，
自定义工具，skill，MCP
灵活的子代理，
有git 可正常做项目等我开放代码后可以用来架构自我升级，
工具包管理，
定时任务，
知识库：(RAG  or  关键词检索  or  图谱)还没想好怎么搞，但你可以让它干，
自维护memory，
多渠道，
本地模型支持及提供精炼记录为语料进行微调(太麻烦，没测试，半成品)，
一些安全防护，
99%～90%缓存命中率，

```
deepthink --help           # 全部选项 · All options
deepthink setup            # 配置向导 · Setup wizard
deepthink --tui            # TUI 模式
deepthink serve            # HTTP 服务器模式 · Server mode
deepthink "your prompt"    # 单次执行 · Single-shot
```

### TUI 快捷键 · TUI Shortcuts

| 按键 · Key     | 功能 · Action                                             |
| -------------- | --------------------------------------------------------- |
| `Ctrl+C`       | 退出 · Exit                                               |
| `Ctrl+L`       | 清屏 · Clear screen                                       |
| `Ctrl+P`       | 切换 Provider · Toggle provider                           |
| `/plan "任务"` | Plan 模式 — 分步执行 · Step-by-step execution             |
| `/spec "任务"` | Spec 模式 — 规格→任务→验收 · spec → tasks → checklist     |
| `/done`        | 停用当前模式 · Deactivate mode                            |
| `/restart`     | 重启（session 自动恢复） · Restart with session preserved |
| `/model`       | 模型选择菜单 · Model selection menu                       |

---

## 配置 · Configuration

运行时配置通过 `update_config` 工具即时生效（热加载），或直接编辑 `.agent/config.json`。

_Runtime config takes effect instantly via `update_config` tool, or edit `.agent/config.json` directly._

> **仅 `channels` 配置需要重启**，其他变更均即时生效。
> _Only `channels` config requires restart. All other changes are instant._

### 飞书渠道 · Feishu Channel

编辑 `.agent/config.json`：

_Edit `.agent/config.json`:_

```json
{
  "channels": {
    "feishu": {
      "enabled": true,
      "appId": "cli_xxxxxxxxxxxx",
      "appSecret": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
      "domain": "feishu"
    }
  }
}
```

配置写入后调用 `restart` 工具或输入 `/restart`，session 自动恢复。飞书渠道在重启后自动连接。

_After writing config, call `restart` tool or type `/restart`. Session is auto-restored, feishu connects on restart._

### MCP 服务器 · MCP Servers

添加 server 定义到 `.agent/mcp.json` — 热加载，无需重启。

_Add server definitions to `.agent/mcp.json` — hot-loaded, no restart needed._

### 自定义 Skill · Custom Skills

将 `.md` 文件放入 `.agent/skills/`：

_Drop `.md` files in `.agent/skills/`:_

```markdown
---
name: my-skill
description: 代码审查辅助 · Code review helper
tools: read, grep
---

分析以下代码中的潜在问题：

_Analyze the following code for issues:_

{{code}}
```

热加载 — 调用 `use_skill(skill_name: "my-skill", variables: {code: "..."})` 即可使用。

_Hot-loaded — call `use_skill` to activate._

---

## 目录结构 · Directory Structure

```
~/.agent/                    # 全局配置和数据 · Global config & data
├── config.json              # 运行时配置 · Runtime config
├── .env                     # API Keys
├── mcp.json                 # MCP 服务器定义
├── skills/                  # 自定义 Skill .md 文件
├── sessions/                # 对话历史 · Conversation history
└── persona/                 # Agent 身份文件 · Identity files

<project>/.agent/            # 项目级覆写 · Project overrides
├── config.json
├── skills/
└── agents.json              # 子 Agent 定义
```

---

## 环境要求 · Requirements

- Node.js >= 18
- pnpm（推荐）或 npm · _pnpm (recommended) or npm_

## 文档 · Docs

`docs/` 目录下有详细文档：

_Detailed guides in `docs/`:_

| 文件              | 内容                             |
| ----------------- | -------------------------------- |
| `user-guide.md`   | 完整用户手册 · Full user manual  |
| `architecture.md` | 架构概览 · Architecture overview |
| `api.md`          | API 参考 · API reference         |
