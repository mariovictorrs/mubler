---
name: mubler-go-development
description: Use when writing, reviewing or organizing Go code in mubler (package structure, errors, logging, tests, dependencies).
---

# Go development in mubler

## Structure

- `cmd/mubler/` entrypoint; everything else under `internal/` (`server`, `listen`, `storage`, `relay`, `stats`).
- Dependencies point inward: `server` → domain → `storage` interfaces. The domain imports neither HTTP nor SQL.

## Conventions

- Standard library first (`net/http` with `ServeMux`, `log/slog`, `encoding/json`); add a dependency only with justification.
- `context.Context` as the first parameter of I/O operations.
- Wrap errors with `fmt.Errorf("...: %w", err)`; use sentinel/typed errors to map to HTTP codes.
- Define small interfaces on the consumer side.
- Configuration via environment variables (`MUBLER_*`); never log tokens.
- Code, identifiers and documentation in English.

## Quality

- `gofmt`, `go vet` and `go test ./...` must pass.
- Table-driven tests; use `httptest` for handlers; in-memory/temporary database for tests.
- Relevant decisions → new ADR in `docs/adr/`.
