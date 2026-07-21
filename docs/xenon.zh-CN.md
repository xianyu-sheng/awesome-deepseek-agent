[English](./xenon.md) | [简体中文](./xenon.zh-CN.md) · [← 返回](../README.zh-CN.md)

# 接入 Xenon

[Xenon](https://github.com/xianyu-sheng/Xenon) 是一个开源终端 AI 编程 Agent，支持 DeepSeek V4 模型发现、可配置推理强度、原生工具调用、缓存与费用观测、工具权限控制、MCP 服务以及八种执行引擎。

## 1. 安装

安装 Python 3.10 或更高版本，然后安装 Xenon 当前源码版本：

```bash
pip install -U "git+https://github.com/xianyu-sheng/Xenon.git"
xenon --version
```

如需参与开发，可克隆仓库后使用 `-e ".[dev]"` 安装。

## 2. 配置 DeepSeek

启动 Xenon 并打开交互式配置向导：

```text
> /setup
```

选择 **DeepSeek**，输入从 [DeepSeek 开放平台](https://platform.deepseek.com/api_keys) 获取的 API Key，再选择 `deepseek-v4-pro` 和/或 `deepseek-v4-flash`。Xenon 会通过供应商的 `/models` API 获取当前模型列表，并将所选模型加入故障转移池。

也可以直接编辑 `~/.xenon/credentials.yaml`：

```yaml
deepseek: "sk-xxxxxxxxxxxxxxxx"
```

Xenon 内置的 V4 模型条目采用 100 万 Token 上下文窗口。上下文管理器会在达到配置上限前预警并压缩长对话。

### 推理强度

`deepseek-v4-pro` 默认使用 `reasoning_effort=max`。可针对单个模型修改：

```text
> /set_model ds-pro deepseek/deepseek-v4-pro reasoning_effort=high
```

允许值为 `low`、`medium`、`high` 和 `max`。该配置会保存到模型注册表，并用于普通请求、流式请求和原生工具请求。当 API 要求强制指定 `tool_choice` 时，由于两者不兼容，Xenon 仅对该次请求关闭思考模式。

## 3. 运行

```bash
cd /path/to/project
xenon
```

常用命令和快捷键：

```text
/models        查看已注册模型
/model         交互式选择一个已注册模型
/pool          查看故障转移池和模型健康状态
/mode react    切换到 ReAct 执行引擎
/cost          查看 Token 缓存用量和预估费用
Shift+Tab      循环切换执行模式
Ctrl+O         展开或折叠执行详情
```

Xenon 会在多轮工具调用中保留 DeepSeek 原生的 `reasoning_content`、`tool_calls` 和 `tool_call_id` 消息。文件及命令工具在执行前会经过权限策略检查。

## 缓存与费用观测

状态栏、`/cost` 视图和退出摘要基于 API 返回的 `usage` 字段，在本地计算缓存命中率与预估费用，不会为展示数据额外发起模型请求。配置细节请参阅 [DeepSeek 缓存指南](https://github.com/xianyu-sheng/Xenon/blob/main/docs/deepseek-guide.md)。

## 视觉桥接器（可选）

按 `Ctrl+Alt+V` 可附加图片。当模型池中已配置的供应商支持视觉输入时，Xenon 会让该模型描述图片，再将文字描述交给 DeepSeek 推理。此功能复用已有供应商凭证，并不表示 DeepSeek 模型本身具备多模态能力。
