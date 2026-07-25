---
name: Submit a rating request to the Origami Risk Standard Rating service
description: >-
  Rate one or more intakes against a rater using Origami's standalone Standard Rating
  API — synchronously for small requests, asynchronously with a requestId poll for
  larger ones — and handle the per-intake error shapes correctly.
api: openapi/origami-risk-standard-rating-api-openapi.json
generated: '2026-07-25'
method: generated
source: openapi/origami-risk-standard-rating-api-openapi.json
operations:
  - POST /Requests/sync
  - POST /Requests/async
  - GET /Requests/{requestId}
  - DELETE /Requests/{requestId}
---

# Standard Rating service

Grounded in `openapi/origami-risk-standard-rating-api-openapi.json` — the only
substantive OpenAPI definition Origami Risk publishes. The spec declares no
`operationId`s, so operations are referenced by method and path exactly as they appear in
the document.

## Before you start

The published `servers[]` entry (`http://origamirater-standard.dev.dais.com?oas`) is an
internal development host, not a customer base URL, and it is plain HTTP. Use the rating
host your Origami tenant is provisioned with. See
`overlays/origami-risk-standard-rating-api-overlay.yaml`.

## Request shape

`StandardRatingRequestDto`:

- `raterId` — **required**. The rater to run.
- `raterVersion` — optional, nullable; pins a rater version.
- `intakeIds` — array of intake references already held by the platform.
- `intakes` — array of inline intake payloads (strings).
- `generateWorksheet` — boolean; produce a rating worksheet.

Content types accepted: `application/json`, `text/json`, `application/*+json`.

## Steps

1. **Small or interactive requests — rate synchronously.** `POST /Requests/sync` returns
   `200` with `SyncRatingResponseDto`:
   - `requestId`
   - `ratingResults[]` — one `RatingResultDetailsDto` per intake, each with `intakeId`,
     `raterResult` and `errors[]`
   - `failedToQueueErrors` — map of id → message for intakes that never made it into the
     run
   **A 200 does not mean every intake rated.** Walk `ratingResults[].errors[]` and
   `failedToQueueErrors` before reporting success.
2. **Larger requests — rate asynchronously.** `POST /Requests/async` returns `202` with
   `AsyncRatingResponseDto { requestId }`.
3. **Poll.** `GET /Requests/{requestId}` (uuid) returns the stored request. `404` means
   the id is unknown or expired.
4. **Cancel.** `DELETE /Requests/{requestId}` returns `204` on success, `404` if unknown.

## Errors

`400` and `404` return `ProblemDetails` (`type`, `title`, `status`, `detail`,
`instance`) — the ASP.NET Core RFC 7807 shape, served as `application/json` rather than
`application/problem+json`. See `errors/origami-risk-problem-types.yml`.

## Rules an agent must follow

- No idempotency key: a re-submitted sync or async request is a new rating run. Retrieve
  by `requestId` before re-submitting.
- Treat per-intake errors as first-class — a partially successful batch is the normal
  outcome shape here, not an exception.
- Proposal-side rating inside the core platform is a different surface entirely:
  `POST /api/Quotes/Proposals/{proposalId}/Rating/RunOptions` — see
  `skills/origami-risk-quote-to-bind.md`.

## Related artifacts

- `openapi/origami-risk-standard-rating-api-openapi.json`
- `overlays/origami-risk-standard-rating-api-overlay.yaml`
- `data-model/origami-risk-data-model.yml`
