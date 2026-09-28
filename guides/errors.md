---
title: "SkyCast API errors: codes, causes and fixes"
description: "Every error SkyCast API returns — INVALID_PARAMETER, UNAUTHORIZED, CITY_NOT_FOUND, RATE_LIMITED, UPSTREAM_UNAVAILABLE and more — with its cause and what to do."
status: generated
version: "0.1"
---

# Errors

Every error has the same JSON shape and a matching HTTP status, so one handler covers them all:

```json
{
  "error": {
    "code": "INVALID_PARAMETER",
    "message": "\"days\" must be an integer from 1 to 16.",
    "status": 400,
    "details": { "parameter": "days" },
    "request_id": "7b0e3c1a-2f4d-4f7e-9a51-3c2b8f6d1e90"
  }
}
```

- **`code`** — stable; branch on it
- **`message`** — for people; may change wording
- **`details`** — extra context when there is any, such as the parameter at fault
- **`request_id`** — also in the `X-Request-Id` header; quote it when you report a problem

## Error codes

| Status | `code` | Cause | What to do |
|---|---|---|---|
| 400 | `INVALID_PARAMETER` | A parameter is missing, empty or out of range | Fix the parameter named in `details.parameter` |
| 401 | `UNAUTHORIZED` | No key, or a key the server does not know | Send a valid key in `X-API-Key` or `Authorization: Bearer` |
| 404 | `CITY_NOT_FOUND` | No city matches the name, name + country, or id | Check the spelling, or search with [`/v1/cities`](../reference/cities.md) |
| 404 | `NOT_FOUND` | The path does not exist | Check the URL against the [reference](../reference/README.md) |
| 405 | `METHOD_NOT_ALLOWED` | Not a `GET` | Use `GET` |
| 429 | `RATE_LIMITED` | Too many requests in this window | Wait `Retry-After` seconds — see [rate limits](./caching-and-rate-limits.md) |
| 502 | `UPSTREAM_UNAVAILABLE` | The weather provider did not answer within 5 seconds | Retry after a few seconds |
| 500 | `INTERNAL_ERROR` | A bug on the server | Report it with the `request_id` |

## Retry only what can succeed

Retrying a `400`, `401` or `404` returns the same error. Retry `429` after `Retry-After`, and `502` with a short back-off — two or three attempts a few seconds apart.

## Next steps

<!-- widget:cards plain cols=2 arrow=hover -->

- [Caching and rate limits](./caching-and-rate-limits.md) — Avoid `429` in the first place {gauge}
- [Find the right city](./locations.md) — Avoid `CITY_NOT_FOUND` {map-pin}

<!-- /widget -->
