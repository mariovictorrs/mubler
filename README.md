# mubler

Self-hosted scrobbler that works as a **ListenBrainz-compatible relay**.

Point your client (Navidrome, etc.) at mubler: it stores everything you listen to, optionally forwards listens to the official ListenBrainz, and generates charts and metrics from your history.

> Status: **planning phase**. There is no code yet; the repository only contains the documentation base.

## How it works

```
┌───────────┐  submit-listens   ┌────────────────────────┐   (optional)   ┌──────────────┐
│ Navidrome │ ────────────────▶ │         mubler         │ ─────────────▶ │ ListenBrainz │
│ & others  │   (LB-compat API) │ API ▸ Storage ▸ Stats  │     relay      │  (official)  │
└───────────┘                   └──────────┬─────────────┘                └──────────────┘
                                           │
                                  Dashboard / charts / metrics
```

## Goals

- Receive listens through a ListenBrainz-compatible API (`/1/submit-listens`, `/1/validate-token`, ...).
- Store the complete history locally, with no dependency on external services.
- Optionally relay to the official ListenBrainz, with a queue and retries.
- Generate metrics and charts: top artists/albums/tracks, activity over time, streaks, etc.
- Easy to host: a single binary/container with few dependencies.

### Out of scope (for now)

- Public multi-user sign-up, social features, recommendations.
- Music playback.

## Planned stack

Go (backend) — details and alternatives in [`docs/adr`](docs/adr/README.md).

## Repository layout

```
.
├── docs/
│   ├── architecture.md      # architecture overview
│   └── adr/                 # Architecture Decision Records
├── internal/                # application code (still empty)
│   └── server/
└── .github/skills/          # skills for AI agents
    ├── listenbrainz-compat/
    ├── mubler-go-development/
    └── mubler-metrics/
```

## Documentation

- [Architecture](docs/architecture.md)
- [Architecture decisions (ADRs)](docs/adr/README.md)

## Roadmap

1. **Foundation** — documentation, ADRs, Go project skeleton.
2. **Ingestion** — `submit-listens`, `validate-token`, token auth, storage.
3. **Relay** — queue forwarding to ListenBrainz with retries.
4. **Metrics** — aggregate queries and stats endpoints.
5. **Dashboard** — charts.
6. **Import** — import existing history (ListenBrainz, Last.fm).

## Contributing

Relevant architectural decisions must be recorded as an ADR (see [`docs/adr/README.md`](docs/adr/README.md)) before implementation.

## License

See [LICENSE](LICENSE).
