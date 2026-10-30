# Architecture

Initial overview; subject to change through [ADRs](adr/README.md).

## Components

| Component | Responsibility |
|-----------|----------------|
| **API (LB-compatible)** | Receives listens in ListenBrainz format, authenticates by token, validates payloads. |
| **Storage** | Persists listens and metadata in an append-only, deduplicated way. |
| **Relay** | Durable queue that forwards listens to the official ListenBrainz with retries and backoff. |
| **Stats** | Queries/aggregations over the history (top N, time series, streaks). |
| **Web/Dashboard** | Charts and metrics visualization. |

## Ingestion flow

1. Client sends `POST /1/submit-listens` with `Authorization: Token <token>`.
2. The API validates the token and payload (`listen_type`: `single`, `import`, `playing_now`).
3. `single`/`import` listens are stored (deduplicated by user + `listened_at` + track).
4. If the relay is enabled, a job is enqueued for the official ListenBrainz.
5. `playing_now` is not persisted in the history; it is kept only in memory/cache with a TTL.

## Principles

- **Compatibility first**: responses and error codes follow ListenBrainz.
- **Never lose a listen**: store locally before reporting success; relay failures never affect the client.
- **Idempotency**: resubmissions do not create duplicates.
- **Raw data preserved**: keep the original `track_metadata` (JSON) alongside normalized fields.
- **Operational simplicity**: deployable as a single service.

## Planned code layout

```
cmd/mubler/        # entrypoint
internal/server/   # HTTP, routes, middlewares
internal/listen/   # listen domain
internal/storage/  # persistence
internal/relay/    # forwarding
internal/stats/    # metrics
```
