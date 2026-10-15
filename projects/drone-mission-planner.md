# Drone mission planner — product and technical case study

**Status:** working portfolio prototype. The source repository is private. This document describes the product at a high level without publishing source code or operational data.

## The user decision

Before a drone flight, a pilot needs to bring location, route, weather and aircraft assumptions together. The prototype provides one planning flow: choose a location and planned time, enter mission parameters, review a route on a map, and inspect estimated duration, energy use and conditions that merit manual review.

**Product boundary:** this is an educational planning estimate. It does not check airspace, grant flight clearance, certify aircraft performance or replace manufacturer guidance and local rules.

## What the prototype includes

- Responsive React interface with a Leaflet/OpenStreetMap map.
- Location search or coordinates; device location is requested only when selected.
- Seven-day hourly weather and terrain elevation metadata from Open-Meteo.
- An Express API that validates inputs and returns a deterministic route and energy estimate.
- Review prompts for wind, precipitation, visibility, temperature, darkness and battery reserve.
- Backend tests and a Vercel configuration that routes the frontend and API under one origin.

## System view

```text
Pilot inputs + location + planned time
       ├─ Place search → OpenStreetMap Nominatim
       ├─ Weather and elevation → Open-Meteo
       └─ Mission parameters → Express simulation API
                                  ↓
                  Route + estimates + review prompts
                                  ↓
                       React planning interface
```

The simulation is intentionally deterministic. Inputs such as route bearing, distance, speed, altitude, payload, wind and reserve target produce an inspectable estimate. Review prompts call attention to conditions that need a human check.

## Product choices and trade-offs

| Choice | Why it matters |
| --- | --- |
| One pre-flight workflow | Reduces switching between unrelated tools while forming a plan. |
| Separate data adapters and simulation endpoint | Makes external-service failures and business logic easier to reason about. |
| Explicit review prompts | Keeps the user aware of operating conditions and the model's limits. |
| No claim of flight readiness | The model has not been validated against aircraft-specific field data or safety requirements. |

## API surface

- GET /api/health — service availability
- GET /api/geocode — place search
- GET /api/weather — local current and selected-hour conditions
- POST /api/simulate — validated mission inputs, route, estimates and review items

## What I would validate next

1. Interview pilots about their actual pre-flight decision sequence and trusted information sources.
2. Compare estimated duration and energy with aircraft-specific telemetry across wind, payload and route scenarios.
3. Add editable waypoints and saved plans after the core review flow proves useful.
4. Assess airspace, terrain hazards, privacy, rate limits and operational requirements before broader pilots.

This case study demonstrates product framing, API and data integration, rule design, explicit limitations, and a testable roadmap. It does not claim measured pilot outcomes or production safety readiness.
