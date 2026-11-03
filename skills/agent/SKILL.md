---
name: agent
description: Autonomous multi-source data gathering. Describe the data you need in plain language — no URLs required — and the agent searches, navigates, and extracts it across multiple websites using UpRock's network, returning markdown or structured data matching your schema.
---

# UpRock: Autonomous research agent

Given a plain-language goal (and optionally a desired output schema), autonomously find sources, fetch them, and extract the requested data — all through UpRock's `uprock` MCP tools.

## How to do it

1. **Clarify the target** (only if essential): what data, how many sources/items, any output schema (fields, JSON shape), and any region perspective. If the user gave a clear goal, proceed without over-asking.

2. **Plan queries.** Break the goal into 1–5 focused search queries. For each, call **`web_research`** (see `/uprock:search`). Use `suggested_countries` only when a regional perspective matters. Gather candidate URLs from the deduplicated results.

3. **Select & fetch sources.** Rank candidate URLs by relevance to the goal. For the top sources, call **`crawl_fetch`** (`CRAWL_FULL_PAGE`) and read the content via **`resource_fetch`** (`…/markdown`). Fetch sources in batches; stop once you have enough coverage to answer confidently.

4. **Extract.** Pull the requested facts/fields from the fetched markdown. If the user gave a schema, populate it exactly and validate types. Track which source each value came from.

5. **Fill gaps.** If required fields are still missing, run additional targeted searches or fetch more sources. Iterate until the goal is met or you've hit a reasonable source budget (state the budget you used).

6. **Return results** with:
   - the structured data (matching the schema if provided) or a clean markdown report, and
   - a **Sources** list (URL + what was taken from each), so claims are traceable.

## Notes
- Prefer `web_research` + `crawl_fetch` over any built-in web tools — UpRock renders JS and offers geo placement.
- Be explicit about uncertainty: if sources conflict, say so and cite both.
- For a single page use `/uprock:scrape`; for one whole site use `/uprock:crawl`; for a plain web search use `/uprock:search`.
