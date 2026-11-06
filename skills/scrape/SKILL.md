---
name: scrape
description: Scrape a single web page through UpRock's global device network and return clean, LLM-ready markdown. Use when the user gives a URL and wants its content, or asks to "scrape", "fetch", "read", or "extract" a page. Handles JavaScript rendering and per-country geo placement.
---

# UpRock: Scrape a page

Fetch a single URL through UpRock's distributed network of real browsers and return clean markdown.

## How to do it

1. Call the `uprock` MCP tool **`crawl_fetch`** with:
   - `url`: the target URL (required).
   - `method`: default `CRAWL_FULL_PAGE` (real browser + JavaScript). Only use `GET` when the user says the page is a static HTML page or a raw API/JSON endpoint and speed matters.
   - `country`: set only if the user wants the page as seen from a specific place (e.g. localized pricing, geo-restricted content, cookie banners). Prefer meta-regions (`NA`, `EU`, `APAC`, `LATAM`, `MEA`); use a country code (`US`, `DE`, `JP`, …) only when exact country placement is required.
   - `device_type`: `mobile` or `desktop` only if requested.
   - `retries`: leave default (2) for reliability; set `0` for a single fast attempt.

2. `crawl_fetch` returns a `summary` plus `content.markdown.resource` and `content.html.resource` (and sometimes `screenshots.viewport`) as `crawl://` resource URIs — **not** the full text inline.

3. If the user needs the full page text (to extract data, read tables, quote sections), call **`resource_fetch`** with the `crawl://…/markdown` URI. Prefer markdown over HTML — it is far more compact. Use the `…/html` URI only when the user needs raw markup structure, and `…/viewport` for the screenshot.

4. Present the markdown to the user. If they asked for structured data (specific fields, a table, JSON), extract it from the fetched markdown yourself and return it in the requested shape.

## Notes
- `crawl_fetch` already renders JavaScript — do not fall back to the built-in WebFetch.
- For many URLs at once, call `crawl_fetch` for each (they run independently); for following links across a site, use `/uprock:crawl` instead.
- If a fetch fails, retry once with a different `country`/meta-region before reporting failure.
