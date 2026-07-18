[English](./omniagent.md) | [简体中文](./omniagent.zh-CN.md) · [← Back](../README.md)

# Integrate with OmniAgent

OmniAgent is an open-source, multi-model AI agent scheduling engine that runs in your terminal.

- **GitHub:** <https://github.com/xianyu-sheng/omniagent>

It features **8 inference paradigms** (ReAct, Plan-Execute, Reflection, Direct, Novel, and 3 combined engines), **20+ built-in tools** (file editing, AST analysis, Git, MCP, web fetch), engineering reliability layers (Circuit Breaker, Budget Manager, Context Compactor), and a **MCP ecosystem** with Smithery registry browsing (7000+ servers) and a 27-skill library.

#### 1. Install OmniAgent

- Install [Python 3.10+](https://www.python.org/downloads/).
- Run the following commands in your terminal:

```bash
git clone https://github.com/xianyu-sheng/omniagent.git
cd omniagent
pip install -e ".[dev]"
```

- After installation, run the following command. If the version and help text are displayed, the installation is successful:

```bash
omniagent --version
```

#### 2. Configure DeepSeek Provider

OmniAgent supports DeepSeek natively. Pick one of two ways to configure:

**Method A: Interactive setup (recommended)**

Launch OmniAgent and run the setup wizard:

```text
> /setup
```

Choose **DeepSeek** from the provider list, paste your API Key, and select models (`deepseek-v4-pro` / `deepseek-v4-flash`). The models are automatically added to the priority-based calling pool.

**Method B: Direct config file**

Edit `~/.omniagent/credentials.yaml`:

```yaml
providers:
  deepseek:
    api_key: "sk-xxxxxxxxxxxxxxxx"
```

Get your API Key from the [DeepSeek Platform](https://platform.deepseek.com/api_keys).

> **1M Context Window:** OmniAgent automatically detects the 1M token context window for DeepSeek V4 models. A tier-aware Context Compactor warns at 60% usage and auto-compacts at 85%, preserving semantically dense content with multi-segment structured summaries.

> **Max Thinking / Reasoning Effort:** DeepSeek V4 Pro supports multiple reasoning effort levels. Pass `reasoning_effort` via the engine layer or set it in the model registry. The ReAct and Plan-Execute engines pass through reasoning parameters automatically.

#### 3. Run and Select Model

- Enter your project directory and execute:

```bash
cd /path/to/my-project
omniagent
```

- Use `Shift+Tab` to cycle through inference paradigms, or type `/mode react` to switch.
- Models are auto-selected by the **AutoRouter** based on task difficulty — simple queries use `deepseek-v4-flash`, complex tasks use `deepseek-v4-pro`.
- To force a specific model:

```text
> /models          # list registered models
> /pool            # view the 5-tier priority pool
```

- Start coding with your multi-paradigm terminal agent 🐋
