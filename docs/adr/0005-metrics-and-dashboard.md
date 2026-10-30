# ADR-0005: Metrics and dashboard

- **Status:** Proposed
- **Date:** 2026-10-17

## Context

The project's main value is turning listening history into charts and metrics.

## Decision (proposed)

- Metrics computed with SQL queries over the history, exposed through its own JSON API (`/api/stats/...`), separate from the LB-compatible API.
- Web dashboard served by the binary itself (assets embedded with `embed`), using a charting library on the front end.
- Initial metrics: top artists/albums/tracks per period, listens per day/hour, streaks, total listening time, new artists.
- User time zone taken into account when grouping by day.

## Alternatives considered

- **Grafana + SQL datasource** — flexible, but an external dependency and less integrated UX.
- **Prometheus export** — suitable for operational metrics, not for history.

## Consequences

- Front-end framework choice is left to a later ADR.
- Caching/materialization may be needed as volume grows.
