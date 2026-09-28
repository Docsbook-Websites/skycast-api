---
title: "SkyCast API quickstart: your first forecast in 5 minutes"
description: "Start a SkyCast API server with Node.js or Docker, set an API key, and request the current weather and a 7-day forecast for any city."
status: generated
version: "0.1"
---

# Quickstart

Start a SkyCast server, give it an API key, and ask it for the weather in Paris. You need **Node.js 20+** or **Docker**; nothing else gets installed.

<!-- widget:stepper -->

### Get the code and set a key

Clone the SkyCast API repository, then copy the example config and put your own key in it:

```bash
cd skycast-api
cp .env.example .env
```

In `.env`, set `SKYCAST_API_KEYS` to a key of your choice — for example `SKYCAST_API_KEYS=my-first-key`. Several keys are separated by commas.

### Start the server

With Node.js, load the config and start:

```bash
set -a; source .env; set +a
npm start
```

With Docker, build the image and pass the same file:

```bash
docker build -t skycast .
docker run -p 3000:3000 --env-file .env skycast
```

Either way, the server prints `SkyCast API listening on http://0.0.0.0:3000`.

### Check that it answers

The health endpoint needs no key:

```bash
curl http://localhost:3000/v1/health
```

```json
{ "status": "ok", "version": "1.0.0", "uptime_s": 4 }
```

<!-- /widget -->

## Ask for the current weather

Send the key in the `X-API-Key` header and name a city:

```bash
curl -H "X-API-Key: my-first-key" \
  "http://localhost:3000/v1/weather/current?city=Paris"
```

The response names the city SkyCast resolved and the reading for right now, in local time:

```json
{
  "location": { "id": 2988507, "name": "Paris", "country": "France", "country_code": "FR", "timezone": "Europe/Paris" },
  "current": {
    "time": "2026-09-28T10:30",
    "temperature": 17.8,
    "feels_like": 16.9,
    "condition": { "code": 61, "id": "rain", "text": "Light rain" }
  },
  "units": { "temperature": "°C", "wind_speed": "km/h", "precipitation": "mm", "pressure": "hPa" }
}
```

## Get a 7-day forecast

The [forecast endpoint](./reference/forecast.md) takes the same location parameters, plus `days`:

```bash
curl -H "X-API-Key: my-first-key" \
  "http://localhost:3000/v1/weather/forecast?city=Paris&days=7"
```

You get one entry per day in `daily`, starting today in the city's time zone. Add `hours=24` to get an hourly forecast for the next day as well.

<!-- widget:callout type=warning -->

With `SKYCAST_API_KEYS` empty the server starts with authentication **off** and logs a warning. That is convenient on your laptop and unsafe anywhere else.

<!-- /widget -->

## Next steps

<!-- widget:cards plain cols=2 arrow=hover -->

- [Find the right city](./guides/locations.md) — Country filters, city ids and coordinates {map-pin}
- [Units and languages](./guides/units-and-languages.md) — Fahrenheit, mph and Russian condition text {languages}
- [Weather for many cities](./guides/batch.md) — Up to 10 cities in one request {layout-grid}
- [API reference](./reference/README.md) — Every endpoint with a Try it form {book-open}

<!-- /widget -->
