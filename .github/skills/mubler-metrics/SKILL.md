---
name: mubler-metrics
description: Use when defining, implementing or querying listening metrics and statistics in mubler (top artists, time series, streaks, SQL aggregations).
---

# mubler metrics

## Initial catalog

- Top artists / albums / tracks per period (week, month, year, all time).
- Listens per day, weekday and hour.
- Total listening time (use `duration_ms` when available; otherwise mark as estimated).
- Streaks of consecutive days with listening.
- New artists/tracks discovered per period.

## Rules

- Compute from the persisted history; `playing_now` never counts.
- Group by day in the **user's time zone**, not UTC.
- Artist/album/track identity: prefer MBID; fall back to normalized name (lowercase, trimmed).
- Half-open intervals `[from, to)` in Unix timestamps.
- Endpoints under `/api/stats/...`, separate from the LB-compatible API; JSON responses with explicit period and time zone.
- Queries must use indexes; measure before adding caching/materialization.

## Checklist

- Test with a fixed dataset and a known expected result.
- Cover edge cases: day/time zone boundaries, empty period, duplicate listens.
