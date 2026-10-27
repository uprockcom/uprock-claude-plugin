# UpRock for Claude Code

**Scrape, search, crawl, and sweep the web from a real global device network.**

UpRock turns any website into clean, LLM-ready markdown — with real-browser rendering, anti-bot handling, and per-country geo placement — searches the web across multiple engines and regions at once, and tests uptime and performance from **real devices worldwide**. It includes an autonomous research agent: describe the data you need in plain language and it finds, navigates, and extracts it across multiple sites.

Unlike datacenter-proxy scrapers, UpRock runs on a distributed network of real mobile and desktop devices around the globe, so requests look like genuine local users — and you can place a crawl or search in a specific region to see exactly what locals see.

## Capabilities

| Command | What it does |
| --- | --- |
| `/uprock:scrape` | Extract a single page as clean markdown (JS rendering, anti-bot, geo placement). |
| `/uprock:crawl` | Crawl multiple pages of a site and return their content as markdown. |
| `/uprock:search` | Search the web across multiple engines and countries, deduplicated. |
| `/uprock:sweep` | Test a URL's uptime & performance (Core Web Vitals) from real devices across regions. |
| `/uprock:agent` | Describe the data you need — the agent autonomously searches, fetches, and extracts it. |
| `/uprock:setup` | Configure your API key and verify everything works. |

You can also just ask in plain language — "scrape this URL", "search the web for X", "is my site up worldwide?" — and Claude will use the UpRock tools directly.

## Setup

1. **Install** (from a marketplace that lists this plugin):
   ```
   /plugin install uprock@<marketplace>
   ```
2. **Get your API key** at **https://uprock.ai**: sign up, then create a key at **https://uprock.ai/app/api-keys**. (The MCP setup page at https://uprock.ai/app/mcp also shows your ready-to-use MCP URL.)
3. When prompted at install/enable, paste your key into **UpRock API Key**. (Or later: `/plugin` → **uprock** → **Configure**.) It is stored securely in your OS keychain and injected into the MCP connection automatically — never written to a file.
4. Run `/uprock:setup` to smoke-test the connection.

### Self-hosted / staging
Set the **MCP Base URL** plugin option to point at a different UpRock MCP host (default: `https://mcp.uprock.ai`). The key is appended to the path automatically (`<base>/<api_key>/mcp`).

## How it works

This plugin bundles UpRock's hosted **MCP server** (Streamable HTTP). Your API key is injected via Claude Code's `${user_config.api_key}` substitution into the server URL — `https://mcp.uprock.ai/<api_key>/mcp` — so the tools appear natively in Claude Code:

- `crawl_fetch` — fetch a URL through the device network (returns markdown + HTML as `crawl://` resources)
- `resource_fetch` — read the full content of a `crawl://` or `sweep://` resource
- `web_research` — multi-engine, multi-region web search
- `sweep` — multi-region uptime & performance test

The slash commands above are thin orchestration layers over these tools.

## Roadmap

- **Semantic video search** — UpRock's video search (find the exact moment across indexed video by meaning) is available today via the REST API for early-access partners and will be added here once it ships as an MCP tool. Contact developer@uprock.com for early access.

## Privacy & security

- Your API key is stored in the OS keychain (Claude Code `sensitive` user config), never committed or written to project files.
- Requests are made to your configured UpRock MCP host only.

## Links

- Website & signup: https://uprock.ai
- API keys: https://uprock.ai/app/api-keys
- n8n community node: https://github.com/uprockcom/n8n-nodes-uprock
- Support: developer@uprock.com

## License

MIT © UpRock
