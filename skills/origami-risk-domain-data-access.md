---
name: Read and write Origami Risk domain data safely
description: >-
  Discover the tenant's configurable domains and their field dictionaries, query records
  with the column/filter syntax, and create, upsert and bulk-load records without
  tripping Origami's page, bulk and payload limits.
api: https://developers.origamirisk.com/reference/domains-entities
generated: '2026-07-25'
method: generated
source: https://developers.origamirisk.com/reference/domains-entities
operations:
  - GET /api/Domains
  - GET /api/Metadata/Domains/{domain}/DataDictionary
  - GET /api/Metadata/Domains/{domain}/InputSample
  - GET /api/Metadata/Domains/{domain}/ScreenConfiguration
  - GET /api/{domain}/Fields
  - GET /api/{domain}/Codes
  - GET /api/{domain}/Query
  - GET /api/{domain}/Filters/ReadableText
  - GET /api/{domain}/{id}
  - POST /api/{domain}
  - POST /api/{domain}/Upsert
  - PUT /api/{domain}
  - DELETE /api/{domain}/{id}
  - POST /api/{domain}/BulkInsert
  - POST /api/{domain}/BulkUpsert
  - GET /AccountInformation/Limits
  - GET /AccountInformation/ThreadCheck
  - POST /api/Link/Upsert
  - GET /api/Link/Query
---

# Domain data access on Origami Risk

Origami is a configured platform: nearly every record type is reached generically as a
`{domain}` path segment whose fields differ per tenant. Discover the model at runtime;
do not hard-code it.

## Discover

1. `GET /AccountInformation/Limits` — the tenant's limits. Defaults are 100 records per
   query page and 25 records per bulk insert/upsert, but the reference states the returned
   set varies with Origami configuration.
2. `GET /api/Domains` — every domain in the tenant.
3. `GET /api/Metadata/Domains/{domain}/DataDictionary` — the field-level contract.
   `.../InputSample` gives a sample input object and `.../ScreenConfiguration` gives the
   default columns.
4. `GET /api/{domain}/Fields` and `GET /api/{domain}/Codes` for field and code-list
   detail. `POST /api/{domain}/Codes/Upsert` maintains code values.

Domain names are **case-sensitive** and must match the Domain Name in the system. Client
Defined Entities are addressed with a `Custom.` prefix.

## Query

`GET /api/{domain}/Query` with:

- `columns` — comma-separated list of fields to return, supporting dotted traversal
  (e.g. `Location.Name, ClaimNumber, LossDate`). Omit it and the API returns all the
  fields it normally uses, which is slower and larger.
- The platform's view-filter syntax for the filter. Validate the filter before running it
  (the reference publishes a filter-syntax validation call), render it human-readably with
  `GET /api/{domain}/Filters/ReadableText`, and convert between string and JSON-tree forms
  with `GET /api/Reports/ViewFilterStringToJsonTree` and
  `POST /api/Reports/JsonTreeToViewFilterString`.
- Results are paginated at 100 records per response. Page rather than widening the filter.
- GET URLs must stay under 10,000 characters — chunk large `columns` or filter lists into
  several calls, as the guide recommends.

Single record: `GET /api/{domain}/{id}`.

## Write

- `POST /api/{domain}` creates; `PUT /api/{domain}` updates;
  `POST /api/{domain}/Upsert` creates-or-edits in one call;
  `DELETE /api/{domain}/{id}` deletes.
- `POST /api/{domain}/BulkInsert` and `POST /api/{domain}/BulkUpsert` take an array of
  record objects. **Exceeding the cap raises an exception and processes nothing** — the
  batch is all-or-nothing, and the default cap is 25 records per call.
- `fireEvents` (default `false`) decides whether the write triggers Origami's system
  events and Data Entry Events. Leave it `false` for data loading; set it `true` when the
  write should drive downstream workflow.
- `echoFields` returns a chosen comma-separated set of fields from the written record, so
  you can confirm the write without a second call.
- Attach context with `POST /api/{domain}/{id}/Notes`, `POST /api/{domain}/{id}/Emails`
  and the file-upload endpoints. Relate records with `POST /api/Link/Upsert`,
  `POST /api/Link/Remove` and `GET /api/Link/Query`.

## Rules an agent must follow

- **Upsert is not idempotency.** Origami documents no `Idempotency-Key` header. Upsert
  converges on the record identity you supply — if you supply none, a retry creates a
  duplicate.
- Keep any single request body under 10 MB, including files.
- Check `GET /AccountInformation/ThreadCheck` before driving volume; it reports the
  environment's worker and I/O thread availability.
- Read the response body: parts of this surface report validation problems inside a 200.

## Related artifacts

- `data-model/origami-risk-data-model.yml`
- `rate-limits/origami-risk-rate-limits.yml`
- `conventions/origami-risk-conventions.yml`
