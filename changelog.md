---
title: "SkyCast API changelog"
description: "Changes to SkyCast API by version: new endpoints, parameters and behaviour."
status: generated
version: "0.1"
---

# Changelog

## 1.0.0

The first release.

- **Current weather** — [`GET /v1/weather/current`](./reference/current.md) by city name, `city_id` or coordinates
- **Forecast** — [`GET /v1/weather/forecast`](./reference/forecast.md), up to 16 days daily and 168 hours hourly
- **Batch** — [`GET /v1/weather/batch`](./reference/batch.md), up to 10 cities per call
- **City search** — [`GET /v1/cities`](./reference/cities.md) with country filter
- **Units and languages** — `metric` / `imperial`, `en` / `ru`
- **Operations** — API keys, per-key rate limits, 10-minute weather cache, `/v1/health`, OpenAPI 3.1 at `/openapi.yaml`
