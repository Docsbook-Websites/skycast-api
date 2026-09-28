---
title: "GET /v1/weather/current — current weather for one location"
description: "Reference for the SkyCast current weather endpoint: location parameters, units and language, every response field, and errors."
status: generated
version: "0.1"
---

# Current weather

Returns the conditions right now for one location, chosen by city name, city id or coordinates.

<!-- widget:api -->

## GET /v1/weather/current

Conditions for the location's latest 15-minute reading. Cached for 10 minutes; the `X-Cache` header says whether this answer came from the cache.

| Field | Type | Required | Description |
|---|---|---|---|
| `city` | string | no | City name, e.g. `Paris`. One of `city`, `city_id` or `lat` + `lon` is required. |
| `country` | string | no | ISO 3166-1 alpha-2 code that narrows `city`, e.g. `FR`. |
| `city_id` | integer | no | City id from [`/v1/cities`](./cities.md). Wins over every other location parameter. |
| `lat` | number | no | Latitude, −90 to 90. Send with `lon`. |
| `lon` | number | no | Longitude, −180 to 180. Send with `lat`. |
| `units` | string | no | `metric` (default) or `imperial`. |
| `lang` | string | no | `en` (default) or `ru` — language of names and condition text. |

### Authorization

Send your API key as `Authorization: Bearer <key>` or in the `X-API-Key` header. The server operator sets keys in `SKYCAST_API_KEYS` — see the [quickstart](../quickstart.md#get-the-code-and-set-a-key).

### Example

```bash
curl "http://localhost:3000/v1/weather/current?city=Paris&units=metric" \
  -H "X-API-Key: $SKYCAST_KEY"
```

### Returns

| Field | Type | Description |
|---|---|---|
| `location` | object | The place that was resolved: `id`, `name`, `country`, `country_code`, `region`, `latitude`, `longitude`, `timezone`, `population` |
| `current` | object | The reading — fields below |
| `units` | object | Units of every number: `temperature`, `wind_speed`, `precipitation`, `pressure` |

### `current` fields

| Field | Type | Description |
|---|---|---|
| `time` | string | Local time of the reading |
| `temperature` | number | Air temperature at 2 m |
| `feels_like` | number | Apparent temperature |
| `humidity` | integer | Relative humidity, % |
| `pressure` | number | Sea-level pressure, hPa |
| `wind_speed` | number | Wind at 10 m |
| `wind_direction` | integer | Degrees the wind comes from, 0 = north |
| `wind_gust` | number | Gusts at 10 m |
| `precipitation` | number | Precipitation in the last 15 minutes |
| `cloud_cover` | integer | Cloud cover, % |
| `is_day` | boolean | Whether the sun is up |
| `condition` | object | `code` (WMO), `id` (stable, for icons), `text` (in `lang`) |

### Response

```json
{
  "location": { "id": 2988507, "name": "Paris", "country": "France", "country_code": "FR", "region": "Île-de-France", "latitude": 48.85341, "longitude": 2.3488, "timezone": "Europe/Paris", "population": 2138551 },
  "current": { "time": "2026-09-28T10:30", "temperature": 17.8, "feels_like": 16.9, "humidity": 82, "pressure": 1012.4, "wind_speed": 14.2, "wind_direction": 230, "wind_gust": 31.0, "precipitation": 0.4, "cloud_cover": 96, "is_day": true, "condition": { "code": 61, "id": "rain", "text": "Light rain" } },
  "units": { "temperature": "°C", "wind_speed": "km/h", "precipitation": "mm", "pressure": "hPa" }
}
```

### Use cases

- A weather widget on a travel or events page
- Showing conditions next to a delivery or ride ETA

### Errors

| Status | Meaning |
|---|---|
| `400` | `INVALID_PARAMETER` — a parameter is missing or out of range |
| `401` | `UNAUTHORIZED` — missing or unknown API key |
| `404` | `CITY_NOT_FOUND` — no city matches |
| `429` | `RATE_LIMITED` — wait `Retry-After` seconds |
| `502` | `UPSTREAM_UNAVAILABLE` — the weather provider did not answer |

<!-- /widget -->

## Related

<!-- widget:cards plain cols=2 arrow=hover -->

- [Find the right city](../guides/locations.md) — Which location parameter to use {map-pin}
- [Forecast](./forecast.md) — The next 16 days {calendar-days}

<!-- /widget -->
