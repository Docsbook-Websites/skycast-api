---
title: "GET /v1/weather/forecast — daily and hourly forecast"
description: "Reference for the SkyCast forecast endpoint: up to 16 days of daily forecast and up to 168 hours of hourly forecast for one location."
status: generated
version: "0.1"
---

# Forecast

Returns a daily forecast for up to 16 days and, on request, an hourly forecast for up to 168 hours.

<!-- widget:api -->

## GET /v1/weather/forecast

Days start today in the location's time zone. `hourly` is included only when `hours` is greater than 0.

| Field | Type | Required | Description |
|---|---|---|---|
| `city` | string | no | City name, e.g. `Paris`. One of `city`, `city_id` or `lat` + `lon` is required. |
| `country` | string | no | ISO 3166-1 alpha-2 code that narrows `city`, e.g. `FR`. |
| `city_id` | integer | no | City id from [`/v1/cities`](./cities.md). Wins over every other location parameter. |
| `lat` | number | no | Latitude, −90 to 90. Send with `lon`. |
| `lon` | number | no | Longitude, −180 to 180. Send with `lat`. |
| `days` | integer | no | Days of daily forecast, 1–16, today included. Default `7`. |
| `hours` | integer | no | Hours of hourly forecast, 0–168. Default `0` (no `hourly`). |
| `units` | string | no | `metric` (default) or `imperial`. |
| `lang` | string | no | `en` (default) or `ru` — language of names and condition text. |

### Authorization

Send your API key as `Authorization: Bearer <key>` or in the `X-API-Key` header. The server operator sets keys in `SKYCAST_API_KEYS` — see the [quickstart](../quickstart.md#get-the-code-and-set-a-key).

### Example

```bash
curl "http://localhost:3000/v1/weather/forecast?city=Tokyo&days=3&hours=6" \
  -H "X-API-Key: $SKYCAST_KEY"
```

### Returns

| Field | Type | Description |
|---|---|---|
| `location` | object | The place that was resolved, as in [current weather](./current.md) |
| `daily` | array | One entry per day — fields below |
| `hourly` | array | One entry per hour — only when `hours` > 0 |
| `units` | object | Units of every number |

### `daily` fields

| Field | Type | Description |
|---|---|---|
| `date` | string | Local date, `YYYY-MM-DD` |
| `temperature_min` | number | Lowest temperature of the day |
| `temperature_max` | number | Highest temperature of the day |
| `precipitation_sum` | number | Total precipitation |
| `precipitation_probability` | integer | Highest chance of precipitation during the day, % |
| `wind_speed_max` | number | Strongest wind |
| `uv_index_max` | number | Highest UV index |
| `sunrise` | string | Local time of sunrise |
| `sunset` | string | Local time of sunset |
| `condition` | object | The day's dominant condition |

### `hourly` fields

| Field | Type | Description |
|---|---|---|
| `time` | string | Local hour |
| `temperature` | number | Air temperature |
| `feels_like` | number | Apparent temperature |
| `precipitation` | number | Precipitation in that hour |
| `precipitation_probability` | integer | Chance of precipitation, % |
| `wind_speed` | number | Wind speed |
| `condition` | object | Condition for that hour |

### Response

```json
{
  "location": { "id": 1850147, "name": "Tokyo", "country": "Japan", "country_code": "JP", "timezone": "Asia/Tokyo" },
  "daily": [
    { "date": "2026-09-28", "temperature_min": 19.6, "temperature_max": 26.1, "precipitation_sum": 0.0, "precipitation_probability": 10, "wind_speed_max": 12.3, "uv_index_max": 5.9, "sunrise": "2026-09-28T05:32", "sunset": "2026-09-28T17:30", "condition": { "code": 2, "id": "partly_cloudy", "text": "Partly cloudy" } }
  ],
  "hourly": [
    { "time": "2026-09-28T17:00", "temperature": 23.3, "feels_like": 27.6, "precipitation": 0.0, "precipitation_probability": 15, "wind_speed": 5.0, "condition": { "code": 3, "id": "overcast", "text": "Overcast" } }
  ],
  "units": { "temperature": "°C", "wind_speed": "km/h", "precipitation": "mm", "pressure": "hPa" }
}
```

### Use cases

- A week view in a travel or outdoor-activity app
- An hour-by-hour rain chart for the next day

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

- [Current weather](./current.md) — Conditions right now {thermometer}
- [Units and languages](../guides/units-and-languages.md) — Imperial units and Russian text {languages}

<!-- /widget -->
