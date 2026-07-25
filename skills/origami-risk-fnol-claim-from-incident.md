---
name: Create a claim from an incident (FNOL) on Origami Risk
description: >-
  Take a reported incident through Origami's first-notice-of-loss path — create or
  locate the incident record, convert it into a claim, then drive the downstream
  FirstReport, Reserve, Review and state EDI reporting actions.
api: https://developers.origamirisk.com/reference/actions
generated: '2026-07-25'
method: generated
source: https://developers.origamirisk.com/reference/actions
operations:
  - POST /api/{domain}
  - POST /api/{domain}/Upsert
  - GET /api/Metadata/Domains/{domain}/InputSample
  - POST /api/v1/Actions/CreateClaimFromIncident/{incidentID}
  - POST /api/v2/Actions/Queue/FirstReport/{domain}/{id}
  - POST /api/v2/Actions/Queue/Reserve/{domain}/{id}
  - POST /api/v2/Actions/Queue/Review/{domain}/{id}
  - POST /api/v2/Actions/Queue/RootCause/{domain}/{id}
  - POST /api/v2/Actions/Queue/EDIReport/{domain}/{id}
  - POST /api/v2/Actions/Queue/Note/{domain}/{id}
  - POST /api/v2/Actions/Queue/Task/{domain}/{id}
  - POST /api/{domain}/{id}/Notes
  - POST /api/Files/Upload/{domain}/{id}
---

# FNOL: incident to claim on Origami Risk

The realtime action lives on the `/api/v1/Actions/...` path and the queued actions on
`/api/v2/Actions/Queue/...`. Both are documented in the public reference; the published
OpenAPI definitions do not cover them.

## Steps

1. **Discover the incident shape.** Incident is a configurable domain, so read
   `GET /api/Metadata/Domains/Incident/DataDictionary` and
   `GET /api/Metadata/Domains/Incident/InputSample` for the tenant's actual field set
   before composing a body. Domain names are case-sensitive; client-defined entities are
   prefixed `Custom.`.
2. **Create or locate the incident.** `POST /api/Incident` (generic
   `POST /api/{domain}`) to create, or `POST /api/Incident/Upsert` to converge on an
   existing record. Use the `fireEvents` query parameter deliberately: `false` suppresses
   Origami's system events and Data Entry Events, which is what you want when loading
   historical data and not what you want when reporting a live loss. `echoFields` returns
   named fields from the created record so you do not need a follow-up read.
3. **Convert the incident into a claim.**
   `POST /api/v1/Actions/CreateClaimFromIncident/{incidentID}`.
   - Path parameter `incidentID` (integer) is the source incident.
   - Query parameters: `coverageID` sets the claim's coverage; `ignoreProperties` is a
     comma-separated list of fields that will **not** be copied to the claim.
   - Body is a JSON key/value object matching the schema of the Action being executed.
4. **Drive the downstream actions** against the new claim, each as a queued action of the
   form `POST /api/v2/Actions/Queue/{action}/{domain}/{id}`:
   - `FirstReport` — the first-report-of-injury/loss action.
   - `Reserve` — set or adjust reserves.
   - `Review` / `RootCause` — review and root-cause workflows.
   - `EDIReport` — US state workers' compensation EDI reporting. Origami makes no ACORD,
     AL3, NGDS or IVANS claim anywhere; `EDIReport` is state reporting, not ACORD
     transport (see `conformance/origami-risk-conformance.yml`).
   - `Note`, `Task`, `Email`, `SMS`, `Fax`, `MailMerge`, `StateForm`, `AssignSurvey`,
     `AuditResponse`, `DataUpdate`, `RoundRobin` and the rest of the queued-action set
     follow the same path shape.
5. **Attach evidence.** `POST /api/{domain}/{id}/Notes` for notes and
   `POST /api/Files/Upload/{domain}/{id}` (or the external-storage variant
   `POST /api/Files/UploadExternal/{domain}/{id}`) for documents and photos. Total request
   body, files included, must stay under 10 MB.

## Rules an agent must follow

- **Queued actions are fire-and-forget.** The `/api/v2/Actions/Queue/...` call enqueues
  work; it does not return the completed result. Poll the affected record rather than
  assuming completion.
- **No idempotency key exists.** Re-posting `CreateClaimFromIncident` for the same
  incident may create a second claim. Read the incident's linked claim first.
- Check bodies, not just statuses — the platform returns validation problems inside 200
  responses on parts of this surface.
- The action body must match the schema of the specific Action being executed; use the
  domain metadata endpoints to resolve it rather than guessing field names.

## Related artifacts

- `data-model/origami-risk-data-model.yml` — Incident → Claim relationship
- `conventions/origami-risk-conventions.yml` — queued-action pattern, generic domains
- `rate-limits/origami-risk-rate-limits.yml` — payload and bulk limits
