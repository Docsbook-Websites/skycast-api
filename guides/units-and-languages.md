---
title: "Units and languages in SkyCast API: metric, imperial, en, ru"
description: "Switch SkyCast API between metric and imperial units, and get city names and weather condition text in English or Russian."
status: generated
version: "0.1"
---

# Units and languages

Two query parameters work on every weather and city endpoint: `units` picks the measurement system, `lang` picks the language of names and condition text.

## Units

| `units` | Temperature | Wind speed | Precipitation | Pressure |
|---|---|---|---|---|
| `metric` (default) | °C | km/h | mm | hPa |
| `imperial` | °F | mph | in | hPa |

Every response repeats the units it used in a `units` block, so your UI can print the right suffix without remembering the request:

```json
"units": { "temperature": "°F", "wind_speed": "mph", "precipitation": "in", "pressure": "hPa" }
```

## Languages

`lang=en` (default) or `lang=ru` changes two things:

- **City, country and region names** — `Москва`, `Россия` instead of `Moscow`, `Russia`
- **`condition.text`** — `Переменная облачность` instead of `Partly cloudy`

```bash
curl -G -H "X-API-Key: $SKYCAST_KEY" \
  "http://localhost:3000/v1/weather/current" \
  --data-urlencode "city=Москва" -d lang=ru
```

## Map conditions to your own icons

`condition.id` never changes with the language, so build icons and colours on it rather than on the text:

| `condition.id` | Meaning |
|---|---|
| `clear`, `mostly_clear` | Sun |
| `partly_cloudy`, `overcast` | Clouds |
| `fog` | Fog or rime |
| `drizzle`, `freezing_drizzle` | Drizzle |
| `rain`, `heavy_rain`, `freezing_rain` | Rain |
| `showers`, `heavy_showers` | Rain showers |
| `snow`, `heavy_snow`, `snow_showers` | Snow |
| `thunderstorm`, `thunderstorm_hail` | Thunderstorm |
| `unknown` | No reading |

`condition.code` carries the underlying WMO weather code if you need finer detail.

## Next steps

<!-- widget:cards plain cols=2 arrow=hover -->

- [Current weather reference](../reference/current.md) — Every field in the answer {thermometer}
- [Forecast reference](../reference/forecast.md) — Daily and hourly fields {calendar-days}

<!-- /widget -->
