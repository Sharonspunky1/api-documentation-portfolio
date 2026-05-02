# Open-Meteo Forecast API

## Overview
A free, open-source weather forecast API that returns daily weather data for any location using geographic coordinates. No authentication is required.

---

## Base URL
https://api.open-meteo.com/v1/forecast

---

## Endpoint
GET /forecast

https://api.open-meteo.com/v1/forecast

This endpoint retrieves a 7-day daily weather forecast for a specified location using latitude and longitude coordinates. Pass your target location's coordinates along with the daily weather variables you want returned. The response includes maximum temperature, minimum temperature, and maximum wind speed organised as parallel arrays by date.

No API key or authentication is required.

---

## Query Parameters

| Parameter | Type | Description |
|----------|------|-------------|
| latitude | float | Latitude in decimal degrees (e.g., 37.763283) |
| longitude | float | Longitude in decimal degrees (e.g., -122.41286) |
| daily | string | Comma-separated list of forecast variables |
| temperature_unit | string | Temperature unit (e.g., fahrenheit) |
| timezone | string | Timezone (e.g., America/Los_Angeles or auto) |

Detailed parameter values and examples are shown below.

---

## Response Description

A successful request returns an HTTP **200 OK** status code and a JSON object. The table below describes every field returned in the response.

---

## Top-level fields

| Field | Type | Description |
|------|------|-------------|
| latitude | float | The latitude of the location used to generate the forecast. This may differ slightly from the requested value due to grid snapping. |
| longitude | float | The longitude of the location used to generate the forecast. This may differ slightly from the requested value due to grid snapping. |
| generationtime_ms | float | The time taken by the server to generate the response, measured in milliseconds. Useful for performance monitoring. |
| utc_offset_seconds | integer | The UTC offset of the requested timezone expressed in seconds. For example, America/Los_Angeles at GMT-7 returns -25200. |
| timezone | string | The full timezone identifier used in the response. For example, America/Los_Angeles. |
| timezone_abbreviation | string | The abbreviated label for the timezone. For example, GMT-7. |
| elevation | float | The elevation of the location in metres above sea level, based on a digital elevation model. |
| daily_units | object | An object describing the unit of measurement applied to each returned daily variable. |
| daily | object | The forecast data organised as parallel arrays. Each index position across all arrays corresponds to the same calendar day. |

---

## daily_units object

| Field | Type | Description |
|------|------|-------------|
| time | string | The format used for date values. Always returns iso8601. |
| temperature_2m_max | string | The unit for maximum temperature values. Returns °F or °C depending on the temperature_unit parameter. |
| temperature_2m_min | string | The unit for minimum temperature values. Returns °F or °C depending on the temperature_unit parameter. |
| windspeed_10m_max | string | The unit for maximum wind speed values. Returns km/h by default unless overridden by the windspeed_unit parameter. |

---

## daily object

The daily object contains parallel arrays where each index corresponds to a single forecast day. Index 0 across all arrays always represents the current day.

| Field | Type | Description |
|------|------|-------------|
| time | array of string | The forecast dates in ISO 8601 format (YYYY-MM-DD). Returns 7 dates by default. |
| temperature_2m_max | array of float | The maximum air temperature at 2 metres above ground for each forecast day. |
| temperature_2m_min | array of float | The minimum air temperature at 2 metres above ground for each forecast day. |
| windspeed_10m_max | array of float | The maximum wind speed at 10 metres above ground for each forecast day. |

---

## HTTP status codes

| Code | Description |
|------|-------------|
| 200 OK | The request succeeded. Forecast data is returned in the response body. |
| 400 Bad Request | The request is missing a required parameter or contains an invalid value. |
| 429 Too Many Requests | The request rate limit has been exceeded. Wait before retrying. |
| 500 Internal Server Error | An unexpected error occurred on the server. Retry the request after a short delay. |

---

## Example Request
GET https://api.open-meteo.com/v1/forecast?latitude=37.763283&longitude=-122.41286&daily=temperature_2m_max,temperature_2m_min,windspeed_10m_max

---

## Example Response

```json
{
  "latitude": 37.763283,
  "longitude": -122.41286,
  "generationtime_ms": 0.065,
  "utc_offset_seconds": -25200,
  "timezone": "America/Los_Angeles",
  "timezone_abbreviation": "GMT-7",
  "elevation": 8.0,

  "daily_units": {
    "time": "iso8601",
    "temperature_2m_max": "°F",
    "temperature_2m_min": "°F",
    "windspeed_10m_max": "km/h"
  },

  "daily": {
    "time": [
      "2026-05-02",
      "2026-05-03",
      "2026-05-04",
      "2026-05-05",
      "2026-05-06",
      "2026-05-07",
      "2026-05-08"
    ],
    "temperature_2m_max": [59.7, 59.0, 62.1, 63.4, 65.0, 64.8, 62.7],
    "temperature_2m_min": [51.9, 51.5, 54.6, 53.3, 54.6, 54.0, 53.6],
    "windspeed_10m_max": [23.6, 21.3, 18.4, 20.5, 22.8, 18.8, 23.5]
  }
}
```

## Response Schema

```json
{
  "latitude": "float",
  "longitude": "float",
  "generationtime_ms": "float",
  "utc_offset_seconds": "integer",
  "timezone": "string",
  "timezone_abbreviation": "string",
  "elevation": "float",

  "daily_units": {
    "time": "string",
    "temperature_2m_max": "string",
    "temperature_2m_min": "string",
    "windspeed_10m_max": "string"
  },

  "daily": {
    "time": ["string"],
    "temperature_2m_max": ["float"],
    "temperature_2m_min": ["float"],
    "windspeed_10m_max": ["float"]
  }
}
```
