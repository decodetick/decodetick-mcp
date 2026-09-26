<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/decodetick/decodetick-mcp/main/docs/assets/logo-dark.svg">
  <img alt="Decodetick MCP" src="https://raw.githubusercontent.com/decodetick/decodetick-mcp/main/docs/assets/logo.svg" width="440">
</picture>

# Decode the tick. Find the edge.

Market intelligence for your AI assistant - algo-trading strategies, analytics and specialized research knowledge.

[![PyPI](https://img.shields.io/pypi/v/decodetick-mcp)](https://pypi.org/project/decodetick-mcp/)
[![License: MIT](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![Python 3.14+](https://img.shields.io/badge/python-3.14%2B-blue)](https://python.org)

[![Add to Cursor](https://cursor.com/deeplink/mcp-install-dark.svg)](https://cursor.com/install-mcp?name=decodetick&config=eyJ1cmwiOiJodHRwczovL21jcC5kZWNvZGV0aWNrLmNvbS9tY3AifQ%3D%3D)
[![Install in VS Code](https://img.shields.io/badge/VS_Code-Install_decodetick-0098FF?logo=githubcopilot)](https://insiders.vscode.dev/redirect/mcp/install?name=decodetick&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fmcp.decodetick.com%2Fmcp%22%7D)

</div>

> ### Just ask
>
> - *"What strategy works best on AAPL?"*
> - *"Show me the top strategy's backtest - return, max drawdown, win rate."*
> - *"How risky is BTC?"*
> - *"How do I avoid overfitting a backtest?"*

## Getting started

**Standard config** works in most MCP clients:

```json
{
  "mcpServers": {
    "decodetick": {
      "url": "https://mcp.decodetick.com/mcp"
    }
  }
}
```

<details>
<summary><b>Cursor</b></summary>

Click the **Add to Cursor** button above, or add to `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "decodetick": {
      "url": "https://mcp.decodetick.com/mcp"
    }
  }
}
```
</details>

<details>
<summary><b>VS Code</b></summary>

Click the **Install in VS Code** button above, use the CLI:

```bash
code --add-mcp '{"name":"decodetick","type":"http","url":"https://mcp.decodetick.com/mcp"}'
```

or add to `.vscode/mcp.json`:

```json
{
  "servers": {
    "decodetick": {
      "type": "http",
      "url": "https://mcp.decodetick.com/mcp"
    }
  }
}
```
</details>

<details>
<summary><b>Claude Code</b></summary>

```bash
claude mcp add --transport http decodetick https://mcp.decodetick.com/mcp
```

Add `--scope user` to enable it in every project.
</details>

<details>
<summary><b>Claude Desktop / claude.ai</b></summary>

**Settings → Connectors → Add custom connector**, then enter:

```
https://mcp.decodetick.com/mcp
```

Connectors are account-level, so the server is available in both the desktop
app and claude.ai. Sign in with Decodetick when prompted, or skip it to use the
anonymous tier.
</details>

<details>
<summary><b>ChatGPT</b></summary>

Custom MCP connectors need **Developer mode** (Plus/Pro/Team/Enterprise/Edu):

1. **Settings → Connectors → Advanced** - enable *Developer mode*.
2. **Settings → Connectors → Create** - name it `Decodetick`, set the MCP
   server URL to `https://mcp.decodetick.com/mcp`, pick *OAuth* (or
   *No authentication* for the anonymous tier), and create.
3. In a chat, open **+ → Developer mode** and toggle Decodetick on.
</details>

<details>
<summary><b>Self-hosted (PyPI / Docker)</b></summary>

```bash
uvx decodetick-mcp             # run straight from PyPI
pip install decodetick-mcp     # or install
docker run -p 8080:8080 $(docker build -q .)  # or containerized
```

Defaults to stdio transport; set `MCP_TRANSPORT=streamable-http` to serve
HTTP. See [Self-host with your own key](#self-host-with-your-own-key) to run
as your subscription tier.
</details>

## How it works

You ask in plain language. The AI picks the right tool. You get an answer with
evidence - strategy metrics you can rank, or research passages you can cite -
not a data dump.

| You ask                                        | You get                                                             |
| ---------------------------------------------- | ------------------------------------------------------------------- |
| *"What strategy works best on AAPL?"*          | Strategies backtested on AAPL, ranked by their score on it          |
| *"How risky is BTC?"*                          | The instrument dossier - volatility, drawdowns, character fingerprint |
| *"What does the research say about position sizing?"* | Ranked passages with `slug` + `locale` for citation          |

## Tools

One tool per pillar. All four are read-only.

- **leaderboard** - the strategy leaderboard: published, backtested
  strategies, ranked. Sort by score or APY - `apy_desc` ranks by `cagr_usd`
  (annualised return in USD, the cross-market canon; local `cagr` is the
  fallback); filter by family or by instrument symbol ("what works best on
  AAPL?").
- **strategy** - one strategy's full dossier: metrics, risk mode, parameters,
  and the backtest summary.
- **instrument** - the catalog of covered markets, or one instrument's
  dossier: buy-and-hold facts and the six-axis character fingerprint.
- **research** - one search across the research: articles plus, on paid
  tiers, the deeper knowledge base. Returns cited passages.

## Example prompts

**Explore the leaderboard**
```
What strategy works best on AAPL?
Show me the highest-APY strategy and its max drawdown.
Which strategy family performs best?
```

**Dig into a strategy**
```
Give me the full details on the MA Crossover strategy.
What's the win rate and risk mode of your top strategy?
```

**Explore the markets**
```
Which markets do you cover?
How risky is BTC - volatility, drawdowns, character?
```

**Search the research**
```
How do I avoid overfitting a backtest?
Find articles about position sizing and drawdown control.
```

## Self-host with your own key

The hosted server at `https://mcp.decodetick.com/mcp` authenticates to the data
API automatically. When you self-host the package, supply your own Decodetick API
key so calls run as your subscription tier instead of the anonymous tier:

1. Ask for a key at contact@decodetick.com (format `dtk_` + 40 hex). It maps to
   your account's tier.
2. Provide it via the `DECODETICK_API_KEY` environment variable - never inline in
   committed config. In an MCP client, reference it as `${env:DECODETICK_API_KEY}`.
3. A malformed value is ignored (requests fall back to anonymous); the active
   posture is logged at startup, prefix only - the key is never logged.

Precedence: `DECODETICK_API_KEY` (self-host) > anonymous. The hosted server
authenticates automatically.

## License

MIT - [decodetick.com](https://decodetick.com)
