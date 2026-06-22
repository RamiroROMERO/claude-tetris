---
name: clima
description: Use this skill when the user asks about the weather, clima, temperature, forecast, lluvia, or says "/clima". Fetches current weather for a local or specified location using the free wttr.in service (no API key needed).
---

When the user invokes this skill, follow these steps:

## 1. Determine the location

- If the user provided a city or location in their message, use it.
- If no location was given, default to **Comayagua, Honduras** (or ask the user for their city if the context makes it unclear).

## 2. Fetch the weather

Use PowerShell to call the `wttr.in` API — no API key required:

```powershell
# Replace CITY with the URL-encoded city name (spaces → +)
Invoke-RestMethod "https://wttr.in/CITY?format=j1" | ConvertTo-Json -Depth 5
```

For a quick one-liner summary use:
```powershell
Invoke-RestMethod "https://wttr.in/CITY?format=3"
```

The `format=j1` endpoint returns JSON with:
- `current_condition[0]` → temp_C, humidity, weatherDesc, windspeedKmph, FeelsLikeC
- `weather[0]` → today's forecast (astronomy, hourly)
- `weather[1]` / `weather[2]` → next two days

## 3. Present the results

Report in a clean, readable format (in Spanish):

```
Clima para <Ciudad> — <fecha>
────────────────────────────
Condición:       Parcialmente nublado
Temperatura:     24 °C  (sensación: 26 °C)
Humedad:         65%
Viento:          18 km/h
────────────────────────────
Pronóstico mañana: 22–28 °C, Soleado
Pasado mañana:     20–25 °C, Lluvia ligera
```

## 4. Error handling

- If the city is not found, `wttr.in` returns an error message — inform the user and ask them to try a different city name.
- If there's no internet or the service is down, say so clearly and suggest they check [wttr.in](https://wttr.in) manually.

## Notes

- `wttr.in` is a free, open-source service — no authentication needed.
- Supports city names, airports (e.g. `MEX`), or coordinates (`19.43,-99.13`).
- All output text should be in **Spanish** to match the project's UI language convention.
