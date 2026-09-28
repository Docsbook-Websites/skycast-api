---
title: "GET /v1/weather/batch — current weather for up to 10 cities"
description: "Reference for the SkyCast batch endpoint: current weather for up to 10 city names in one request, with a result per city."
status: generated
version: "0.1"
---

# Batch

Returns the current weather for up to 10 cities in one request. Each city is resolved on its own, so an unknown name fails only its own entry.

<!-- widget:api -->

## GET /v1/weather/batch

Results come back in the order the names were sent, with duplicates removed. A batch of N cities counts as N requests against the [rate limit](../guides/caching-and-rate-limits.md).

| Field | Type | Required | Description |
|---|---|---|---|
| `cities` | string | yes | Comma-separated city names, 1–10, e.g. `Paris,Tokyo,New York`. |
| `units` | string | no | `metric` (default) or `imperial`. |
| `lang` | string | no | `en` (default) or `ru` — language of names and condition text. |

### Authorization

Send your API key as `Authorization: Bearer <key>` or in the `X-API-Key` header. The server operator sets keys in `SKYCAST_API_KEYS` — see the [quickstart](../quickstart.md#get-the-code-and-set-a-key).

### Example

```bash
curl "http://localhost:3000/v1/weather/batch?cities=Paris,Tokyo,Atlantis" \
  -H "X-API-Key: $SKYCAST_KEY"
```

### Returns

| Field | Type | Description |
|---|---|---|
| `count` | integer | Number of cities after duplicates were removed |
| `results` | array | One entry per city: `query`, `ok`, and either `data` or `error` |

### `results` fields

| Field | Type | Description |
|---|---|---|
| `query` | string | The name as you sent it |
| `ok` | boolean | `true` when this city resolved |
| `data` | object | Same shape as a [current weather](./current.md) response |
| `error` | object | The error for this city, e.g. `CITY_NOT_FOUND` |

### Response

```json
{
  "count": 3,
  "results": [
    { "query": "Paris", "ok": true, "data": { "location": { "name": "Paris", "country_code": "FR" }, "current": { "temperature": 17.8, "condition": { "id": "rain", "text": "Light rain" } } } },
    { "query": "Tokyo", "ok": true, "data": { "location": { "name": "Tokyo", "country_code": "JP" }, "current": { "temperature": 23.4, "condition": { "id": "overcast", "text": "Overcast" } } } },
    { "query": "Atlantis", "ok": false, "error": { "code": "CITY_NOT_FOUND", "message": "No city matches \"Atlantis\".", "status": 404 } }
  ]
}
```

### Limitations

- At most 10 cities per request
- City names only — use [current weather](./current.md) with `city_id` or coordinates for exact places

### Errors

| Status | Meaning |
|---|---|
| `400` | `INVALID_PARAMETER` — `cities` is missing, empty or has more than 10 names |
| `401` | `UNAUTHORIZED` — missing or unknown API key |
| `429` | `RATE_LIMITED` — not enough requests left for every city |
| `502` | `UPSTREAM_UNAVAILABLE` — the weather provider did not answer |

<!-- /widget -->

## Related

<!-- widget:cards plain cols=2 arrow=hover -->

- [Weather for many cities](../guides/batch.md) — How to use batch well {layout-grid}
- [Current weather](./current.md) — One location, any form {thermometer}

<!-- /widget -->
