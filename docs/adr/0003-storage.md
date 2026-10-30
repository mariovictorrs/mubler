# ADR-0003: Storage strategy

- **Status:** Proposed
- **Date:** 2026-10-17

## Context

Expected volume (one or a few users) is tens of thousands to a few million listens. We need aggregate queries over time ranges and simple operations.

## Decision (proposed)

Start with **SQLite** (WAL mode) behind a repository interface in `internal/storage`, allowing a later move to **PostgreSQL**.

- Append-only listens table with a unique deduplication key (`user_id`, `listened_at`, normalized artist, normalized track).
- Keep the original `track_metadata` as JSON plus normalized columns (artist, album, track, MBIDs, duration).
- Versioned migrations from the start.

## Alternatives considered

- **PostgreSQL from day one** — more robust, but adds an operational dependency.
- **TimescaleDB/ClickHouse** — overkill for the volume.

## Consequences

- Trivial deployment (a single database file).
- Heavy aggregations may require indexes/materialized tables.
- The SQLite driver choice (CGO vs. pure Go) should be settled in its own ADR.
