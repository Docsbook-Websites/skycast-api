---
title: "Caching and rate limits in SkyCast API"
description: "How long SkyCast API caches weather and city lookups, how the per-key rate limit works, and how to read the X-Cache and X-RateLimit headers."
status: generated
version: "0.1"
---

# Caching and rate limits

SkyCast caches upstream answers so repeated questions are fast, and limits how many requests each key can make so one client cannot starve the others.

## Caching

| What | Kept for | Setting |
|---|---|---|
| Weather (current and forecast) | 10 minutes | `SKYCAST_CACHE_TTL_S` |
| City lookups and search | 24 hours | `SKYCAST_GEO_CACHE_TTL_S` |

The cache key is the exact upstream question — place, units and forecast length — so `units=imperial` and `units=metric` for the same city are cached separately. Identical requests that arrive at the same moment share a single upstream call.

Weather endpoints tell you what happened in the `X-Cache` header:

- **`X-Cache: HIT`** — served from the cache, no upstream call
- **`X-Cache: MISS`** — fetched just now

## Rate limits

Each API key may make **60 requests per 60-second window** by default. The operator changes it with `SKYCAST_RATE_LIMIT` and `SKYCAST_RATE_WINDOW_S`; `SKYCAST_RATE_LIMIT=0` turns limiting off.

Every limited response carries three headers:

| Header | Meaning |
|---|---|
| `X-RateLimit-Limit` | Requests allowed per window |
| `X-RateLimit-Remaining` | Requests left in this window |
| `X-RateLimit-Reset` | When the window resets, Unix seconds |

Over the limit, you get `429 RATE_LIMITED` and a `Retry-After` header in seconds:

```json
{ "error": { "code": "RATE_LIMITED", "message": "Rate limit exceeded. Retry in 12 s.", "status": 429, "details": { "retry_after_s": 12 } } }
```

## What counts

- **One request** — each call to `current`, `forecast` or `cities`
- **One per city** — each city in a [batch](./batch.md)
- **Nothing** — `/v1/health` and `/openapi.yaml`

## Stay under the limit

1. **Cache on your side too.** The weather changes every 15 minutes at most; asking more often returns the same reading.
2. **Batch a dashboard** instead of calling per city.
3. **Wait `Retry-After` seconds** on a `429` before trying again — retrying at once only fails again.

## Next steps

<!-- widget:cards plain cols=2 arrow=hover -->

- [Errors](./errors.md) — Every error code and what to do {triangle-alert}
- [Weather for many cities](./batch.md) — Fewer calls for a dashboard {layout-grid}

<!-- /widget -->
