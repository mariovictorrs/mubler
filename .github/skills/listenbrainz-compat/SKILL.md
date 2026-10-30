---
name: listenbrainz-compat
description: Use when implementing or changing the ListenBrainz-compatible API endpoints in mubler (submit-listens, validate-token, payload format, authentication, errors).
---

# ListenBrainz compatibility

Reference: https://listenbrainz.readthedocs.io/en/latest/users/api/core.html — always check the official docs before assuming behavior.

## Target endpoints (`/1/` prefix)

- `POST /1/submit-listens`
- `GET /1/validate-token`
- `GET /1/user/{user}/listens` (later phase)

## Rules

- Auth: `Authorization: Token <token>`; missing/invalid token → `401`.
- `submit-listens` body:
  ```json
  {"listen_type": "single", "payload": [{"listened_at": 1700000000,
    "track_metadata": {"artist_name": "...", "track_name": "...", "release_name": "...",
      "additional_info": {"recording_mbid": "...", "duration_ms": 0}}}]}
  ```
- `listen_type`: `single` (1 item), `import` (many), `playing_now` (no `listened_at`, never persisted in history).
- `artist_name` and `track_name` are required and non-empty; `listened_at` is required except for `playing_now`.
- Success: `200 {"status": "ok"}`. Error: `{"code": <http>, "error": "<message>"}`, with `400` for invalid payloads.
- Preserve the original `track_metadata`; ignore unknown fields without failing.
- Idempotency: resubmitting the same listen must not duplicate it.

## Checklist

- Table-driven tests with valid and invalid payloads.
- Test with a real client (Navidrome: `ListenBrainz.BaseURL`).
- Record an ADR if diverging from official behavior.
