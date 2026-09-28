---
title: "SkyCast API reference: endpoints, parameters and responses"
description: "Reference for every SkyCast API endpoint — current weather, forecast, batch, city search and health — with authentication, base URL and the OpenAPI 3.1 spec."
status: generated
version: "0.1"
---

# API reference

SkyCast API is a JSON-over-HTTP API with five endpoints, all `GET`. Each page below documents one endpoint and has a **Try it** form that sends a real request from your browser.

<!-- widget:endpoints -->

- [Current weather](./current.md) `GET /v1/weather/current` — Conditions right now for one location.
- [Forecast](./forecast.md) `GET /v1/weather/forecast` — Daily forecast up to 16 days, hourly up to 168 hours.
- [Batch](./batch.md) `GET /v1/weather/batch` — Current weather for up to 10 cities in one call.
- [Search cities](./cities.md) `GET /v1/cities` — Find a city and its `city_id`.
- [Health](./health.md) `GET /v1/health` — Liveness check, no key needed.

<!-- /widget -->

## Base URL

The base URL is wherever your SkyCast server runs. The examples use a local server:

```text
http://localhost:3000
```

## Authentication

Send your key on every request except `/v1/health`, either as a Bearer token or in the `X-API-Key` header:

```bash
curl -H "Authorization: Bearer $SKYCAST_KEY" "http://localhost:3000/v1/weather/current?city=Oslo"
curl -H "X-API-Key: $SKYCAST_KEY" "http://localhost:3000/v1/weather/current?city=Oslo"
```

The `api_key` query parameter works too, for a quick test in a browser tab. Keys are set by the server operator in `SKYCAST_API_KEYS` — see the [quickstart](../quickstart.md).

## OpenAPI spec

The full contract is OpenAPI 3.1. Every running server serves it at `/openapi.yaml`, so a client generator always reads the version it talks to.

## Conventions

- **Times** are local to the location's `timezone`, ISO 8601 without an offset: `2026-09-28T10:30`
- **Nullable fields** are `null`, never missing — except `hourly`, which appears only when you ask for it
- **Errors** share one shape — see [Errors](../guides/errors.md)

## Next steps

<!-- widget:cards plain cols=2 arrow=hover -->

- [Quickstart](../quickstart.md) — Run a server to try these against {rocket}
- [Errors](../guides/errors.md) — Every error code {triangle-alert}

<!-- /widget -->
