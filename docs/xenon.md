[English](./xenon.md) | [简体中文](./xenon.zh-CN.md) · [← Back](../README.md)

# Integrate with Xenon

Xenon is an open-source terminal AI coding agent with **8 reasoning engines**, **DeepSeek cache optimization**, MCP protocol, and Smithery integration.

- **GitHub:** <https://github.com/xianyu-sheng/Xenon>

**🆕 DeepSeek Cache Tracker** — Xenon ships with a built-in cache hit rate tracker for DeepSeek API. A three-layer monitoring system (real-time toolbar → `/cost` panel → exit savings report) shows your cache hit rate, estimated cost, and total savings — all computed locally from the API response `usage` fields, **zero extra LLM cost**. Full guide: [DeepSeek Cache Best Practices](https://github.com/xianyu-sheng/Xenon/blob/main/docs/deepseek-guide.md)

It also features **8 inference paradigms** (ReAct, Plan-Execute, Reflection, Direct, Novel, and 3 combined engines), **20+ built-in tools** (file editing, AST analysis, Git, MCP, web fetch), engineering reliability layers (Circuit Breaker, Budget Manager, Context Compactor), and a **MCP ecosystem** with Smithery registry browsing (7000+ servers) and a 28-skill library.

#### 1. Install Xenon

- Install [Python 3.10+](https://www.python.org/downloads/).
- Run the following commands in your terminal:

```bash
git clone https://github.com/xianyu-sheng/Xenon.git
cd Xenon
pip install -e ".[dev]"
```

- After installation, verify with:

```bash
xenon --version
```

#### 2. Configure DeepSeek Provider

Xenon supports DeepSeek natively. Pick one of two ways to configure:

**Method A: Interactive setup (recommended)**

Launch Xenon and run the setup wizard:

```text
> /setup
```

Choose **DeepSeek** from the provider list, paste your API Key, and select models (`deepseek-v4-pro` / `deepseek-v4-flash`). Models are automatically added to the priority-based calling pool.

**Method B: Direct config file**

Edit `~/.xenon/credentials.yaml`:

```yaml
deepseek: "sk-xxxxxxxxxxxxxxxx"
```

Get your API Key from the [DeepSeek Platform](https://platform.deepseek.com/api_keys).

> **1M Context Window:** Xenon automatically detects the 1M token context window for DeepSeek V4 models. A tier-aware Context Compactor warns at 60% usage and auto-compacts at 85%, preserving semantically dense content with multi-segment structured summaries.

> **Max Thinking / Reasoning Effort:** DeepSeek V4 Pro supports multiple reasoning effort levels. Pass `reasoning_effort` via the engine layer or set it in the model registry. The ReAct and Plan-Execute engines pass through reasoning parameters automatically.

#### 3. Run and Select Model

- Enter your project directory and execute:

```bash
cd /path/to/my-project
xenon
```

- Use `Shift+Tab` to cycle through inference paradigms, or type `/mode react` to switch.
- Models are auto-selected by the **AutoRouter** based on task difficulty — simple queries use `deepseek-v4-flash`, complex tasks use `deepseek-v4-pro`.
- To force a specific model:

```text
> /models          # list registered models
> /pool            # view the 5-tier priority pool
```

- Type `/cost` anytime to see your DeepSeek cache hit rate and cost breakdown.
- Start coding with your multi-paradigm terminal agent 🐋
