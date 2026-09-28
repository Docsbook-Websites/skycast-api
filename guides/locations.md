---
title: "Find the right city: names, countries, city ids and coordinates"
description: "Tell SkyCast API which place you mean by city name, by name plus country code, by a city_id from city search, or by latitude and longitude — and which one wins."
status: generated
version: "0.1"
---

# Find the right city

Every weather endpoint takes a location in one of three forms: a **city name**, a **city id**, or **coordinates**. Pick the one that matches what your user typed.

| You have | Send | Example |
|---|---|---|
| A name | `city` | `city=Berlin` |
| A name that exists in several countries | `city` + `country` | `city=Paris&country=US` |
| An exact place picked from a list | `city_id` | `city_id=2988507` |
| A GPS position | `lat` + `lon` | `lat=48.8534&lon=2.3488` |

## By city name

`city` accepts any spelling the geocoder knows — `München`, `Munich`, `Москва`, `São Paulo`. SkyCast uses the most relevant match, which is usually the largest city with that name.

```bash
curl -H "X-API-Key: $SKYCAST_KEY" \
  "http://localhost:3000/v1/weather/current?city=Springfield"
```

The `location` block in the answer says which Springfield that was. Check `country_code` and `region` before you show the result to a user.

## Narrow a name with a country

Add `country` with a two-letter ISO 3166-1 code to keep only matches in that country:

```bash
curl -H "X-API-Key: $SKYCAST_KEY" \
  "http://localhost:3000/v1/weather/current?city=Paris&country=US"
```

An unknown combination returns `404 CITY_NOT_FOUND`, not a city in another country.

## Pin an exact city with `city_id`

For a city picker, search once with [`/v1/cities`](../reference/cities.md), show the results, and store the `id` of the one the user chose:

```bash
curl -H "X-API-Key: $SKYCAST_KEY" \
  "http://localhost:3000/v1/cities?q=Springfield&country=US&limit=3"
```

```json
{
  "query": "Springfield",
  "count": 3,
  "results": [
    { "id": 4409896, "name": "Springfield", "region": "Missouri", "country_code": "US", "population": 170188 },
    { "id": 4250542, "name": "Springfield", "region": "Illinois", "country_code": "US", "population": 114394 }
  ]
}
```

Then ask for the weather by id — it never changes meaning, whatever the user typed:

```bash
curl -H "X-API-Key: $SKYCAST_KEY" \
  "http://localhost:3000/v1/weather/forecast?city_id=4250542&days=3"
```

## By coordinates

Pass `lat` and `lon` together, in decimal degrees. The answer's `location` then has `id` and `name` set to `null`, because no city was looked up, and `timezone` comes from the forecast.

```bash
curl -H "X-API-Key: $SKYCAST_KEY" \
  "http://localhost:3000/v1/weather/current?lat=-33.8688&lon=151.2093"
```

## When you send more than one

SkyCast uses exactly one location per request, in this order:

1. **`city_id`** — if present, everything else is ignored
2. **`lat` + `lon`** — if both are present
3. **`city`**, narrowed by `country`

<!-- widget:callout type=note -->

Sending `lat` without `lon` (or the reverse) is a `400 INVALID_PARAMETER`, even when `city` is also present.

<!-- /widget -->

## Next steps

<!-- widget:cards plain cols=2 arrow=hover -->

- [Search cities](../reference/cities.md) — The `/v1/cities` reference {search}
- [Units and languages](./units-and-languages.md) — Get city names in Russian {languages}

<!-- /widget -->
