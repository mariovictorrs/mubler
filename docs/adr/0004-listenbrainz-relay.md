# ADR-0004: Relay to the official ListenBrainz

- **Status:** Proposed
- **Date:** 2026-10-17

## Context

mubler acts as a relay: besides storing, the user may want to keep the official ListenBrainz in sync.

## Decision (proposed)

- **Optional** relay, configurable per user (official ListenBrainz token stored securely).
- Store the listen locally first; forwarding is **asynchronous** through a durable queue in the database itself (outbox).
- Retries with exponential backoff; honor `429` and `X-RateLimit-*` headers.
- Permanent failures (e.g. `400`, `401`) mark the job as failed without blocking others.

## Alternatives considered

- **Synchronous proxy** — couples client availability to the external service.
- **External broker (Redis/NATS)** — unnecessary dependency at this stage.

## Consequences

- The client never suffers from ListenBrainz downtime.
- External tokens are secrets: never log them, and consider encryption at rest.
