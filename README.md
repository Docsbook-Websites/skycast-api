---
title: "SkyCast API — weather for any city, in one REST call"
description: "Current weather, 16-day forecasts, hourly data and city search for any city in the world. Open source, zero dependencies, deploys in one command."
status: generated
version: "0.1"
---

<!-- widget:hero size=large -->

**Weather API**

# Weather for any city, in one REST call

SkyCast API turns a city name into current conditions, a 16-day forecast and hourly data — with one response shape, a cache in front and API keys built in. Open source, zero dependencies, yours to run.

[Get your first forecast](./quickstart.md) · [API reference](./reference/README.md)

```bash
curl -H "X-API-Key: $SKYCAST_KEY" \
  "http://localhost:3000/v1/weather/current?city=Paris"
```

One request in, one normalised JSON out — the same shape for Paris, Tokyo or 48.85, 2.35.

<!-- /widget -->

<!-- widget:stats cols=4 -->

- **16 days** — of daily forecast per call
- **168 h** — of hourly forecast on request
- **10 cities** — in one batch request
- **0** — runtime dependencies to install

<!-- /widget -->

## Everything a weather feature needs

Most weather integrations stall on the same three things: finding the right city, reshaping someone else's payload, and not getting rate-limited. SkyCast handles all three.

<!-- widget:bento -->

- **Ask by name, get the right city** — Send `city=Paris`, narrow it with `country=US`, or pin an exact place with a `city_id` from [city search](./guides/locations.md). Cyrillic, accents and local names work as typed. {badge:City resolution} {span:7} {map-pin}

- **One shape everywhere** — Every endpoint returns the same `location`, the same `condition` object with a stable `id`, and a `units` block, so your UI code never branches per city. {span:5} {braces}

- **Forecast by the day or by the hour** — Up to 16 days of highs, lows, rain probability, UV and sunrise, plus up to 168 hours of hourly data in the same call. {span:4} {calendar-days}

- **A whole dashboard in one call** — [Batch](./guides/batch.md) returns the current weather for up to 10 cities, and one bad name never fails the rest. {span:4} {layout-grid}

- **Fast on the second request** — Weather is cached for 10 minutes and city lookups for 24 hours; the `X-Cache` header tells you which you got. {span:4} {zap}

- **Production guard rails included** — API keys, per-key [rate limits](./guides/caching-and-rate-limits.md) with `X-RateLimit-*` headers, a request id on every response and [one error format](./guides/errors.md). {span:12} {side} {tags} {badge:Built in}

  - API keys
  - Rate limits
  - CORS
  - Request ids
  - Health check
  - OpenAPI 3.1

<!-- /widget -->

## Call it from anything

The API is plain HTTPS and JSON. Here is the current weather in Tokyo, in Fahrenheit:

<!-- widget:code-group -->

```bash
curl -H "X-API-Key: $SKYCAST_KEY" \
  "http://localhost:3000/v1/weather/current?city=Tokyo&units=imperial"
```

```javascript
const res = await fetch(
  "http://localhost:3000/v1/weather/current?city=Tokyo&units=imperial",
  { headers: { "X-API-Key": process.env.SKYCAST_KEY } },
);
const { location, current } = await res.json();
console.log(`${location.name}: ${current.temperature}°F, ${current.condition.text}`);
```

```python
import os, requests

res = requests.get(
    "http://localhost:3000/v1/weather/current",
    params={"city": "Tokyo", "units": "imperial"},
    headers={"X-API-Key": os.environ["SKYCAST_KEY"]},
)
data = res.json()
print(f"{data['location']['name']}: {data['current']['temperature']}°F, {data['current']['condition']['text']}")
```

<!-- /widget -->

The answer names the city it resolved, the reading, and the units it used:

```json
{
  "location": { "id": 1850147, "name": "Tokyo", "country": "Japan", "country_code": "JP", "timezone": "Asia/Tokyo" },
  "current": {
    "time": "2026-09-28T17:30",
    "temperature": 73.9,
    "feels_like": 81.6,
    "humidity": 88,
    "wind_speed": 3.1,
    "condition": { "code": 61, "id": "rain", "text": "Light rain" }
  },
  "units": { "temperature": "°F", "wind_speed": "mph", "precipitation": "in", "pressure": "hPa" }
}
```

## Start where you are

<!-- widget:cards feature cols=2 -->

- [Quickstart](./quickstart.md) — Run the server and get your first forecast in five minutes. {rocket} {color:green}
- [Find the right city](./guides/locations.md) — Names, countries, city ids and coordinates, and which one wins. {map-pin} {color:blue}
- [API reference](./reference/README.md) — Every endpoint, parameter and response, with a Try it form. {book-open} {color:purple}
- [Errors and limits](./guides/errors.md) — Every error code, what caused it and what to do next. {shield-check} {color:amber}

<!-- /widget -->

## Questions developers ask first

<!-- widget:accordion -->

### Where does the weather data come from?

From [Open-Meteo](https://open-meteo.com), which blends national weather-service models. SkyCast resolves the city, calls it, caches the answer and returns it in one stable shape.

### Do I need to install anything?

Node.js 20 or newer, or Docker. SkyCast has no npm dependencies, so `npm start` runs it straight from a clone. See the [quickstart](./quickstart.md).

### How do I tell Paris, France from Paris, Texas?

Pass `country=FR` or `country=US` with the name, or look the city up once in [`/v1/cities`](./reference/cities.md) and use its `city_id`. [Find the right city](./guides/locations.md) covers every option.

### Which units and languages are supported?

`metric` (°C, km/h, mm) and `imperial` (°F, mph, in); city names and condition text in English (`en`) and Russian (`ru`). See [units and languages](./guides/units-and-languages.md).

### What happens when I send too many requests?

You get `429 RATE_LIMITED` with a `Retry-After` header. The default is 60 requests per minute per key, and the operator can change it. See [caching and rate limits](./guides/caching-and-rate-limits.md).

### Is there an OpenAPI spec?

Yes — OpenAPI 3.1. Every SkyCast server serves it at `/openapi.yaml`, so you can generate a client in any language from the exact version you talk to.

<!-- /widget -->

<!-- widget:cta -->

**Five minutes from now**

## Put live weather in your app today

Clone, start, and call your first city — no account, no dependencies.

[Start the quickstart](./quickstart.md) · [Browse the API](./reference/README.md)

<!-- /widget -->
