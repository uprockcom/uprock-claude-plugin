---
name: sweep
description: Test a URL's uptime, reliability, and performance from real devices across geographic regions. Use when the user wants to check if a site is up, how fast it loads worldwide, Core Web Vitals, or to verify a deployment from different regions.
---

# UpRock: Sweep (global uptime & performance)

Load a URL from real devices in multiple regions at once and report reliability + performance. Great right after a deploy, or to see how a site performs for users around the world.

## How to do it

1. Call the `uprock` MCP tool **`verify`** with (`sweep` is its old name and still works):
   - `url`: target URL (required).
   - `regions`: subset of `NA`, `EU`, `APAC`, `LATAM`, `MEA`. Default `[NA, EU, APAC]` (fastest, most reliable pools). Add `LATAM`/`MEA` when the user cares about those markets.
   - `device`: `mobile` (default) or `desktop`.
   - `tries`: checks per region (default 5). All checks run concurrently.
   - `timeout`: overall seconds (default 60); total wall-clock ≈ this value.

2. Each completed check returns Core Web Vitals (TTFB, FCP, LCP, CLS), load time, transfer size, and HTTP protocol. The response includes a `report_url` (shareable human-readable report) and `screenshots` as `sweep://` resource URIs.

3. Summarize for the user:
   - Up/down per region and the `failed_jobs` count (inspect per-job `error` fields for failures).
   - Performance per region (highlight slowest regions / poor LCP or CLS).
   - Share the `report_url`.
   - If they want to *see* a region's render, call **`resource_fetch`** on a `sweep://…/screenshot` URI.

## Notes
- A sweep can partially succeed; always report how many checks failed and why.
- For reading a page's content (not measuring it), use `/uprock:scrape`.
