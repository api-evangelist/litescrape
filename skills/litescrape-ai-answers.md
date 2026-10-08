---
generated: '2026-10-07'
method: generated
name: litescrape-ai-answers
description: Check what Google's AI Overview and AI Mode say for a query, including which domains they cite, to audit brand or site presence in AI answers.
api: openapi/litescrape-openapi.yml
operations: [google_ai_overview, google_ai_mode]
source: >-
  Grounded in openapi/litescrape-openapi.yml (OpenAPI 3.1.0) and
  https://litescrape.com/docs/google-ai-overview, /docs/google-ai-mode and the
  AI Overview Checker tool page; operationIds verified verbatim in the spec.
---

# AI Overview and AI Mode answers

## Auth
- `Authorization: Bearer ls_live_...`. Both MCP tools (`google_ai_overview`, `google_ai_mode`) require a key; there is no keyless allowance for them.

## Steps
1. **AI Overview only** — `google_ai_overview` (`GET /api/google/ai-overview`). Send `q` (or `ludocid` / `kgmid`) with the same localization as Google Search (`location` | `uule` | `lat`+`lon`, `gl`, `hl`, `google_domain`, `device`). The response carries just the AI Overview module as ordered blocks with cited sources; use `google_search` instead when you also need the organic page.
2. **AI Mode** — `google_ai_mode` (`GET /api/google/ai-mode`). Send `q` with `google_domain`, `gl`, `hl`, `device`; set `continuable` when you want a follow-up-capable session. Answers come back as typed text blocks with citations.
3. **Audit** — compare the cited domains against your own; rerun the same `q` set on a schedule to track change.

## Rules
- Each call is one billed request on success; Google may return no AI module for a query, which is still a 200.
- Query parameters and responses are retained unless Zero Data Retention is enabled on a paid key (`PATCH /api/keys/zdr`).
- Keep concurrency at or below the key's `concurrency_limit`.
