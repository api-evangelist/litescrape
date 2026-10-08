---
generated: '2026-10-07'
method: generated
name: litescrape-maps-and-reviews
description: Find Google Maps places for a query or area, then fetch and page through a place's reviews by place_id or data_id.
api: openapi/litescrape-openapi.yml
operations: [google_maps, google_reviews, google_maps_popular_times]
source: >-
  Grounded in openapi/litescrape-openapi.yml (OpenAPI 3.1.0) and
  https://litescrape.com/docs/google-maps, /docs/google-maps-reviews; operationIds verified
  verbatim in the spec; conventions per conventions/litescrape-conventions.yml.
---

# Google Maps places and reviews

## Auth
- `Authorization: Bearer ls_live_...`. On the MCP server `google_maps` is free (50 calls per network per UTC day) and `google_reviews` needs a key.

## Steps
1. **Search places** — `google_maps` (`GET /api/google/maps`). Send `q` with a `location`, `ll` or `lat`+`lon`, optionally `z` (zoom) and `type`. Each page returns 20 places with rating, review count, address and identifiers (`place_id`, `data_id`).
2. **Fetch reviews** — `google_reviews` (`GET /api/google/reviews`). Exactly one of `place_id` or `data_id` is required; `sort_by`, `topic_id`, `query` and `num` filter and size the page (up to 100 a call).
3. **Continue** — pass back `pagination.next_page_token` unchanged as `next_page_token`; continuation requests default to ten reviews unless `num` is given.
4. **Optional: live busyness** — `google_maps_popular_times` (`GET /api/google/maps/popular-times`) for the same place.

## Rules
- One call per request regardless of page size; failed requests are not billed.
- Respect `concurrency_limit` (25 by default) across every endpoint sharing the key.
- Reviews and places are third-party data; the Terms forbid using them to "harm, harass, or unlawfully track individuals".
