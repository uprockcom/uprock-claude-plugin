---
description: Configure and verify your UpRock API key, then confirm the scrape / search / sweep tools are working.
---

# UpRock setup

Help the user get connected to UpRock and verify the plugin works.

## Steps

1. **API key.** This plugin reads the API key from its plugin configuration (`api_key`), which Claude Code prompts for when the plugin is enabled and stores securely in the OS keychain.
   - If the user has not set a key yet, tell them to:
     1. Sign up at **https://uprock.ai**.
     2. Create a key at **https://uprock.ai/app/api-keys** and copy the full key (shown once). The MCP setup page at https://uprock.ai/app/mcp also shows the ready-to-paste `https://mcp.uprock.ai/<key>/mcp` URL.
     3. Re-run plugin configuration: `/plugin` → select **uprock** → **Configure**, and paste the key into **UpRock API Key**. (Advanced: set **MCP Base URL** only for staging/self-hosted; otherwise leave the default `https://mcp.uprock.ai`.)
   - The key is injected into the bundled `uprock` MCP server URL automatically — the user never pastes it into a file.

2. **Verify connectivity.** Once a key is set, confirm the `uprock` MCP tools are available (`crawl_fetch`, `web_research`, `sweep`, `resource_fetch`). If they are not, the key is missing/invalid or the plugin needs re-enabling — guide the user back to step 1.

3. **Smoke test.** Run a tiny end-to-end check and report the result:
   - `crawl_fetch` on `https://example.com` with `method: GET`, `retries: 0` → expect `status: success` and a `crawl://` markdown/html resource.
   - Optionally `web_research` with a simple query and `max_results: 3` to confirm search works.

4. **Show what's available.** Briefly list the commands:
   - `/uprock:scrape` — one page → clean markdown
   - `/uprock:crawl` — many pages of a site → markdown
   - `/uprock:search` — multi-engine, multi-region web search
   - `/uprock:sweep` — global uptime & performance test
   - `/uprock:agent` — autonomous multi-source gathering
   Mention they can also just ask in plain language ("scrape this URL", "search the web for…", "is my site up worldwide?").

## Notes
- Never print the API key back to the user or write it to any file in the project.
- If the smoke test fails with an auth error, the key is wrong or out of credits — point them to https://uprock.ai/app/api-keys (keys) and https://uprock.ai/app (credits/usage).
