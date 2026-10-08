---
generated: '2026-10-07'
method: generated
name: litescrape-google-search
description: Run a localized Google Search through Litescrape and page through organic results, the Knowledge Graph and the AI Overview module.
api: openapi/litescrape-openapi.yml
operations: [google_search]
source: >-
  Grounded in openapi/litescrape-openapi.yml (agents.litescrape.com/openapi.json, OpenAPI 3.1.0)
  and https://litescrape.com/docs/google-search; operationId verified verbatim in the spec; auth per
  authentication/litescrape-authentication.yml, errors per errors/litescrape-problem-types.yml,
  limits per rate-limits/litescrape-rate-limits.yml.
---

# Google Search with Litescrape

## Auth
- `Authorization: Bearer ls_live_...` against `https://api.litescrape.com` (the docs' base). The published spec's server, `https://agents.litescrape.com`, is the ZeroClick plan-based storefront in front of the same paths.
- The MCP tool `google_search` (and `search`, its fast mode) is free without a key for 25 calls per network per UTC day.

## Steps
1. **Search** — `google_search` (`GET /api/google/search`). Send `q` (up to 2,048 characters; or `ludocid` / `kgmid` for an entity instead), localize with `gl`, `hl`, `google_domain`, and either `location`, `uule` or `lat`+`lon` (`radius` in meters; these three geography forms conflict). `device` is desktop, tablet or mobile.
2. **Read the groups** — organic results, Knowledge Graph and, when Google generates one, a top-level `ai_overview`; `tbs` filters and `tbm`-style verticals (nws, shop, vid, lcl) narrow the page.
3. **Page** — pass `start` for the next offset; `num` is 1 to 10, the most Google returns on one page.

## Rules
- Every endpoint costs one call at $0.15 per 1,000; only HTTP 200 is billed.
- Keep in-flight requests at or below `concurrency_limit` from `GET /api/keys/status` (25 by default) or expect `429 concurrency`.
- Errors arrive as `{error, error_code, status_code, request_id, retryable}`; fix 4xx before retrying, honour `Retry-After` on a retryable 503, and quote `request_id` to support.
