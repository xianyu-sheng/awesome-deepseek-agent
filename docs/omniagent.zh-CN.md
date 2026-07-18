[English](./omniagent.md) | [简体中文](./omniagent.zh-CN.md) · [← 返回](../README.zh-CN.md)

# 接入 OmniAgent

OmniAgent 是一个开源的多模型 AI Agent 调度引擎，运行在终端内。

- **GitHub：** <https://github.com/xianyu-sheng/omniagent>

内置 **8 种推理范式**（ReAct、Plan-Execute、Reflection、Direct、Novel 及 3 种组合引擎）、**20+ 项内建工具**（文件编辑、AST 分析、Git、MCP、网页抓取）、工程可靠性层（断路器、预算管理器、上下文压缩器），以及 **MCP 生态**（Smithery 注册中心 7000+ 服务器浏览、27 个技能库）。

#### 1. 安装 OmniAgent

- 安装 [Python 3.10+](https://www.python.org/downloads/)。
- 在终端中执行以下命令：

```bash
git clone https://github.com/xianyu-sheng/omniagent.git
cd omniagent
pip install -e ".[dev]"
```

- 安装结束后，执行以下命令，若显示版本号和帮助信息则安装成功：

```bash
omniagent --version
```

#### 2. 配置 DeepSeek 供应商

OmniAgent 原生支持 DeepSeek。选择以下两种方式之一配置：

**方式 A：交互式配置（推荐）**

启动 OmniAgent 并运行配置向导：

```text
> /setup
```

在供应商列表中选择 **DeepSeek**，粘贴 API Key，选择模型（`deepseek-v4-pro` / `deepseek-v4-flash`）。模型将自动加入优先级调用池。

**方式 B：直接编辑配置文件**

编辑 `~/.omniagent/credentials.yaml`：

```yaml
providers:
  deepseek:
    api_key: "sk-xxxxxxxxxxxxxxxx"
```

其中 API Key 在 [DeepSeek 开放平台](https://platform.deepseek.com/api_keys) 获取。

> **100 万 Token 上下文：** OmniAgent 自动识别 DeepSeek V4 模型的 1M Token 上下文窗口。分层上下文压缩器在 60% 用量时预警、85% 时自动压缩，以多段结构化摘要保留语义最密集的内容。

> **Max Thinking / 推理强度：** DeepSeek V4 Pro 支持多级推理强度。通过引擎层透传 `reasoning_effort` 参数，ReAct 和 Plan-Execute 引擎自动传递推理参数。

#### 3. 运行并选择模型

- 进入项目目录并执行：

```bash
cd /path/to/my-project
omniagent
```

- 按 `Shift+Tab` 切换推理范式，或输入 `/mode react` 切换。
- **AutoRouter** 根据任务难度自动选择模型——简单查询用 `deepseek-v4-flash`，复杂任务用 `deepseek-v4-pro`。
- 手动指定模型：

```text
> /models          # 查看已注册模型
> /pool            # 查看五级优先级调用池
```

- 开始与你的多范式终端 Agent 一起编程 🐋
