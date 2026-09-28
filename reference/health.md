---
title: "GET /v1/health — SkyCast API liveness check"
description: "Reference for the SkyCast health endpoint, which needs no API key and is not rate limited — for load balancers and uptime monitors."
status: generated
version: "0.1"
---

# Health

Returns `ok` while the server is up. It needs no API key and does not count against the rate limit, so point load balancers and uptime monitors at it.

<!-- widget:api -->

## GET /v1/health

Answers without calling the weather provider, so it reports whether SkyCast itself is running.

### Example

```bash
curl "http://localhost:3000/v1/health"
```

### Returns

| Field | Type | Description |
|---|---|---|
| `status` | string | Always `ok` |
| `version` | string | Server version |
| `uptime_s` | integer | Seconds since the server started |

### Response

```json
{ "status": "ok", "version": "1.0.0", "uptime_s": 3600 }
```

<!-- /widget -->

## Related

<!-- widget:cards plain cols=2 arrow=hover -->

- [Quickstart](../quickstart.md) — Start a server {rocket}
- [API reference](./README.md) — All endpoints {book-open}

<!-- /widget -->
