---
title: "Weather for many cities in one request with SkyCast API"
description: "Use the SkyCast batch endpoint to get the current weather for up to 10 cities in one call, with a per-city result so one unknown name does not fail the rest."
status: generated
version: "0.1"
---

# Weather for many cities

`GET /v1/weather/batch` returns the current weather for up to **10 cities** in one request — the call behind a dashboard or a list of saved places.

```bash
curl -G -H "X-API-Key: $SKYCAST_KEY" \
  "http://localhost:3000/v1/weather/batch" \
  --data-urlencode "cities=Paris,Tokyo,New York"
```

## Every city gets its own result

Results come back in the order you sent the names. Each one has `ok`; a city that could not be resolved carries its `error` instead of `data`, and the others still succeed:

```json
{
  "count": 2,
  "results": [
    { "query": "Paris", "ok": true, "data": { "location": { "name": "Paris" }, "current": { "temperature": 17.8 } } },
    { "query": "Atlantis", "ok": false, "error": { "code": "CITY_NOT_FOUND", "message": "No city matches \"Atlantis\".", "status": 404 } }
  ]
}
```

The HTTP status is `200` whenever the request itself was valid, so always check `ok` per item.

## Rules

- **1–10 names**, comma-separated; more is a `400 INVALID_PARAMETER`
- **Duplicates are removed** — `Paris,Paris` is one city
- **`units` and `lang`** apply to every city in the batch
- **Names only** — for exact places, call [`/v1/weather/current`](../reference/current.md) with `city_id`

<!-- widget:callout type=info -->

A batch counts as one request **per city** against your [rate limit](./caching-and-rate-limits.md): 10 cities use 10 of your 60 requests a minute.

<!-- /widget -->

## Next steps

<!-- widget:cards plain cols=2 arrow=hover -->

- [Batch reference](../reference/batch.md) — Parameters and response fields {layout-grid}
- [Caching and rate limits](./caching-and-rate-limits.md) — How batches are counted {gauge}

<!-- /widget -->
