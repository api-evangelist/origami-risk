---
name: Quote a risk and bind it into a policy on Origami Risk
description: >-
  Create a proposal (quote), attach lines of business, coverages and schedules, run
  validations and rating, review billing options, then bind the proposal into an active
  policy on the Origami Risk platform.
api: https://developers.origamirisk.com/reference/quotes-and-proposals
generated: '2026-07-25'
method: generated
source: https://developers.origamirisk.com/reference/quotes-and-proposals
operations:
  - POST /api/Quotes/Proposals
  - GET /api/Quotes/InsurancePrograms/{insuranceProgramId}/ListPolicyLines
  - GET /api/Quotes/InsurancePrograms/{insuranceProgramId}/ListStates
  - GET /api/Quotes/InsurancePrograms/{insuranceProgramsId}/ListCarriers
  - PATCH /api/Quotes/Proposals/{id}
  - POST /api/Quotes/Proposals/{proposalId}/ProposalCoverage
  - POST /api/Quotes/Proposals/{proposalId}/Schedules/{domain}
  - POST /api/Quotes/Proposals/{proposalId}/Validations/Run
  - GET /api/Quotes/Proposals/{proposalId}/Validations/Status
  - POST /api/Quotes/Proposals/{proposalId}/Rating/RunOptions
  - POST /api/Quotes/Proposals/{proposalId}/Rating/Queue
  - GET /api/Quotes/Proposals/{proposalId}/Rating/Status
  - GET /api/Quotes/Proposals/{proposalId}/BillingOptions
  - POST /api/Quotes/Proposals/{proposalId}/BindQuote
  - POST /api/Quotes/Proposals/{proposalId}/QueueBindQuote
  - GET /api/Quotes/Proposals/{proposalId}/BindStatus
---

# Quote to bind on Origami Risk

Every path below is documented on the public Origami Risk developer portal. The published
OpenAPI definitions do not cover this surface and declare no `operationId`s, so operations
are identified by method and path exactly as the reference publishes them.

## Before you start

1. Set your environment. The base URL is `https://{environment}.origamirisk.com/OrigamiApi`,
   where `{environment}` is one of `preprod`, `staging`, `live`, `staging-gov`, `live-gov`
   (US) — EU tenants use `origamiriskeu.com` instead. See `sandbox/origami-risk-sandbox.yml`.
2. Get a token: `POST /Authentication/Authenticate` with
   `{Account, ClientName, User, Password}`, or `POST /Authentication/AuthenticateOAuth`
   with `Grant_Type=client_credentials`, `Client_ID="{Account}:{Client}:{User}"`,
   `Client_Secret`. Send the token in the `Token` header on every subsequent call.
   401 means the credential is wrong, not that the endpoint is missing.
3. Read the tenant's limits once: `GET /AccountInformation/Limits`. Query pages cap at
   100 records and bulk writes at 25 per call by default.

## Steps

1. **Resolve the program context.** Use
   `GET /api/Quotes/InsurancePrograms/{insuranceProgramId}/ListPolicyLines`,
   `.../ListStates` and `.../ListCarriers` to find the valid policy lines, states and
   carriers for the program you are quoting.
2. **Create the proposal.** `POST /api/Quotes/Proposals` with a JSON body whose keys are
   fields on the Proposal domain. Pass `policyLineIDs` as a comma-separated query
   parameter (e.g. `"7,12,15"`) to attach lines of business, and `completeLOBs=true` if
   the line-of-business sections should be marked complete.
   - A quote is a Proposal with `Type = "N"`. Other type codes: `A` Application,
     `E` Endorsement, `R` Renewal, `W` Rewrite, `I` Reissue.
   - **This call can return HTTP 200 while carrying validation errors.** Inspect the
     response body before treating the create as successful.
3. **Fill the risk out.** `PATCH /api/Quotes/Proposals/{id}` for header fields,
   `POST /api/Quotes/Proposals/{proposalId}/ProposalCoverage` to add coverages, and
   `POST /api/Quotes/Proposals/{proposalId}/Schedules/{domain}` to add scheduled items
   (locations, vehicles, and so on) for the relevant domain.
4. **Validate.** `POST /api/Quotes/Proposals/{proposalId}/Validations/Run` for the
   synchronous pass, or `POST .../Validations/Queue` then poll
   `GET .../Validations/Status`. Waive a blocking flag deliberately with
   `POST .../Validations/{id}/Waive` (and `.../Unwaive` to reverse it).
5. **Rate.** Use `POST /api/Quotes/Proposals/{proposalId}/Rating/RunOptions`.
   - Do **not** use `POST .../Rating/Run` for new integrations — the reference marks it
     deprecated and points at `RunOptions`.
   - Rating is expensive and can exceed the request timeout; the documented pattern is to
     pair the run with polling `GET .../Rating/Status`, or to use
     `POST .../Rating/Queue` for the asynchronous path.
   - **Rating failures come back on a 200.** Check `RatingStatus`: `"C"` is complete,
     `"E"` is an error and `RatingErrorMessage` carries the reason. On success the same
     object gives you `Premium`, `TotalCost` and `LineItems[]` with `PolicyLineCode` and
     per-line `Premium`.
6. **Choose billing.** `GET /api/Quotes/Proposals/{proposalId}/BillingOptions` (or
   `GET .../EndorsementBillingOptions` when quoting an endorsement).
7. **Bind.** `POST /api/Quotes/Proposals/{proposalId}/BindQuote` synchronously, or
   `POST .../QueueBindQuote` and poll `GET .../BindStatus`.
8. **Issue.** Binding produces a bound proposal; issuing the policy is the Policies
   surface — see `skills/origami-risk-issue-and-service-policy.md`.

## Rules an agent must follow

- **There is no idempotency key.** Origami documents no `Idempotency-Key` header and no
  retry-safety contract. Binding, accepting and payment calls are not safe to blindly
  retry — re-read state (`GET /api/Quotes/Proposals/{id}`, `GET .../BindStatus`) before
  re-issuing any write.
- **Never trust the status code alone.** Proposal creation and rating both return 200 on
  failure paths. Parse the body every time.
- **Prefer the queued variants for anything slow.** Rating, validation and binding all
  have queue + status pairs; use them rather than holding a synchronous request open.
- Field names on a Proposal are tenant-specific. Discover them with
  `GET /api/Metadata/Domains/{domain}/DataDictionary`,
  `GET /api/Metadata/Domains/{domain}/InputSample` and the proposal metadata endpoints
  (`GET /api/Metadata/ProposalFields`, `/ProposalCoverageFields`, `/ProposalScheduleFields`)
  rather than hard-coding them.
- Keep request bodies under 10 MB and GET URLs under 10,000 characters.

## Related artifacts

- `conventions/origami-risk-conventions.yml` — versioning, async patterns, error semantics
- `errors/origami-risk-problem-types.yml` — the error shapes, including 200-with-errors
- `rate-limits/origami-risk-rate-limits.yml` — page size, bulk cap, payload and URL limits
- `authentication/origami-risk-authentication.yml` — token formats and HMAC signing
