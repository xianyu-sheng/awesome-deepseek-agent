[English](./xenon.md) | [简体中文](./xenon.zh-CN.md) · [← Back](../README.md)

# Integrate with Xenon

[Xenon](https://github.com/xianyu-sheng/Xenon) is an open-source terminal AI coding agent. It supports DeepSeek V4 model discovery, configurable reasoning effort, native tool calling, cache and cost observability, permission-gated coding tools, MCP servers, eight execution engines, and transparent user-governed memory.

## 1. Install

Install Python 3.10 or newer, then install the verified v0.7.2 release:

```bash
pip install -U "git+https://github.com/xianyu-sheng/Xenon.git@v0.7.2"
xenon --version
```

For development, clone the repository and install `-e ".[dev]"` instead.

## 2. Configure DeepSeek

Start Xenon and open the interactive setup:

```text
> /setup
```

Choose **DeepSeek**, enter an API key from the [DeepSeek Platform](https://platform.deepseek.com/api_keys), and select `deepseek-v4-pro` and/or `deepseek-v4-flash`. Xenon discovers the provider's current model list through its `/models` API and adds selected models to the fallback pool.

Credentials can also be configured in `~/.xenon/credentials.yaml`:

```yaml
deepseek: "sk-xxxxxxxxxxxxxxxx"
```

Xenon's built-in V4 entries use a 1M-token context window. Its context manager warns and compacts long conversations before the configured limit is reached.

### Reasoning effort

`deepseek-v4-pro` defaults to `reasoning_effort=max`. Change the value for an individual model with:

```text
> /set_model ds-pro deepseek/deepseek-v4-pro reasoning_effort=high
```

Accepted values are `low`, `medium`, `high`, and `max`. The setting is persisted in the model registry and passed through ordinary, streaming, and native-tool requests. When the API requires a forced `tool_choice`, Xenon disables thinking for that individual request because the two options are incompatible.

## 3. Run

```bash
cd /path/to/project
xenon
```

Useful commands and shortcuts:

```text
/models        list registered models
/model         select a registered model interactively
/pool          inspect the fallback pool and model health
/mode react    switch to the ReAct execution engine
/cost          inspect token cache usage and estimated cost
/memory status inspect memory scopes, paths, and budgets
Shift+Tab      cycle execution modes
Ctrl+O         expand or collapse execution details
```

Xenon preserves DeepSeek's native `reasoning_content`, `tool_calls`, and `tool_call_id` messages across tool rounds. File and command tools pass through its permission policy before execution. v0.7.2 also persists bounded, privacy-safe tool checkpoints, including concurrent Plan-Execute steps, so an interrupted session can recover without replaying state-changing work.

## Cache and cost observability

The status bar, `/cost` view, and exit summary derive cache hit rate and estimated cost locally from API `usage` fields. This reporting does not make an additional model request. See the [DeepSeek cache guide](https://github.com/xianyu-sheng/Xenon/blob/main/docs/deepseek-guide.md) for configuration details.

## User-governed memory

Xenon v0.7.2 provides four isolated memory scopes: session, project-local,
project-shared, and user. An explicit request such as `Remember that this project
uses Python 3.12` is saved immediately with a receipt containing its ID, scope,
and exact local path. When Xenon itself detects reusable information, it only
shows a candidate, reason, scope, and destination; nothing is persisted without
the user's confirmation.

Memory is bounded by token budgets, can be inspected with `/memory inspect`, and
checked with `/memory doctor`. Potential conflicts are reported without silent
overwrite; `/memory replace` and `/memory rollback` keep the change reversible.
See the [memory system specification](https://github.com/xianyu-sheng/Xenon/blob/v0.7.2/docs/MEMORY_SYSTEM_SPEC.md) for the storage and consent model.

## Vision Bridge (optional)

Press `Ctrl+Alt+V` to attach an image. When a configured provider in the model pool supports vision, Xenon asks that model to describe the image and supplies the resulting text to DeepSeek for reasoning. This reuses existing provider credentials; it does not make DeepSeek itself multimodal.
