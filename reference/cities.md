---
title: "GET /v1/cities — search cities by name"
description: "Reference for the SkyCast city search endpoint: find cities by name, filter by country, and get the city_id for exact weather lookups."
status: generated
version: "0.1"
---

# Search cities

Returns the cities whose name matches a query, most relevant first. Use a result's `id` as `city_id` in the weather endpoints.

<!-- widget:api -->

## GET /v1/cities

Results are cached for 24 hours. A query with no match returns `count: 0` and an empty list, not an error.

| Field | Type | Required | Description |
|---|---|---|---|
| `q` | string | yes | City name or its beginning, 2–100 characters. |
| `limit` | integer | no | Maximum results, 1–20. Default `5`. |
| `country` | string | no | ISO 3166-1 alpha-2 code to keep only one country, e.g. `US`. |
| `lang` | string | no | `en` (default) or `ru` — language of names. |

### Authorization

Send your API key as `Authorization: Bearer <key>` or in the `X-API-Key` header. The server operator sets keys in `SKYCAST_API_KEYS` — see the [quickstart](../quickstart.md#get-the-code-and-set-a-key).

### Example

```bash
curl "http://localhost:3000/v1/cities?q=Springfield&country=US&limit=3" \
  -H "X-API-Key: $SKYCAST_KEY"
```

### Returns

| Field | Type | Description |
|---|---|---|
| `query` | string | The query as received |
| `count` | integer | Number of results |
| `results` | array | Cities, each with `id`, `name`, `country`, `country_code`, `region`, `latitude`, `longitude`, `timezone`, `population` |

### Response

```json
{
  "query": "Springfield",
  "count": 2,
  "results": [
    { "id": 4409896, "name": "Springfield", "country": "United States", "country_code": "US", "region": "Missouri", "latitude": 37.21533, "longitude": -93.29824, "timezone": "America/Chicago", "population": 170188 },
    { "id": 4250542, "name": "Springfield", "country": "United States", "country_code": "US", "region": "Illinois", "latitude": 39.80172, "longitude": -89.64371, "timezone": "America/Chicago", "population": 114394 }
  ]
}
```

### Use cases

- The autocomplete in a city picker
- Turning an ambiguous name into one `city_id` you can store

### Errors

| Status | Meaning |
|---|---|
| `400` | `INVALID_PARAMETER` — `q` is missing or shorter than 2 characters |
| `401` | `UNAUTHORIZED` — missing or unknown API key |
| `429` | `RATE_LIMITED` — wait `Retry-After` seconds |
| `502` | `UPSTREAM_UNAVAILABLE` — the geocoder did not answer |

<!-- /widget -->

## Related

<!-- widget:cards plain cols=2 arrow=hover -->

- [Find the right city](../guides/locations.md) — Build a city picker with `city_id` {map-pin}
- [Current weather](./current.md) — Use the `city_id` you found {thermometer}

<!-- /widget -->
