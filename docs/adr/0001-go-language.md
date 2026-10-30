# ADR-0001: Go as the backend language

- **Status:** Proposed
- **Date:** 2026-10-17

## Context

mubler is a self-hosted service that must be lightweight, easy to distribute and reliable under concurrent I/O (receiving listens and relaying).

## Decision

Use **Go**, with the standard library (`net/http`, `log/slog`) as the base and minimal dependencies. Application code lives in `internal/`.

## Alternatives considered

- **Rust** — great performance, steeper learning curve and longer development time.
- **Node/TypeScript** — fast to prototype, heavier runtime for self-hosting.
- **Python** — strong data ecosystem, worse distribution story.

## Consequences

- Single binary and small Docker image.
- Good fit with the ecosystem (Navidrome is also Go).
- Charts live in the front end (see ADR-0005), not in Go.
