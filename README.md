# Origami Risk (origami-risk)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Origami Risk is a Chicago-headquartered risk, safety and insurance SaaS company founded in 2009 that began as a single-version cloud RMIS (risk management information system) and grew into a core-systems platform for the United States property and casualty market. It sells policy administration, digital underwriting, rating, billing, premium audit, claims administration, compliance and EHS/GRC modules to carriers, MGAs, program administrators, third-party administrators, risk pools, brokers, healthcare systems and large self-insureds, across workers' compensation, medical professional liability, personal auto and homeowners lines.

Unlike most US insurance organizations, Origami Risk sits in the software layer between carriers and distribution and therefore does publish a genuinely public, self-serve developer portal at [developers.origamirisk.com](https://developers.origamirisk.com/) — a ReadMe-hosted reference readable without login, covering quote and proposal creation, rating, bind, policy issue/endorse/cancel, billing and payments, claims-from-incident and first-report actions, files, reports, domain metadata and outbound webhooks. Four OpenAPI definitions are downloadable from the portal's spec registry, though three of the four are thin scaffolds and the bulk of the reference is hand-authored per-endpoint documentation rather than a complete machine-readable spec. Access to a live tenant is still commercial — the base URL is a per-customer `https://{environment}.origamirisk.com` host and authentication is token-based or HMAC against a provisioned account — and no ACORD, AL3, IVANS or NGDS conformance is claimed anywhere in the marketing site or the developer portal.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/origami-risk/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/origami-risk/refs/heads/main/apis.yml)

## Tags

- Insurance
- United States
- Property and Casualty
- Policy Administration
- Claims
- Underwriting
- Core Systems
- Risk Management
- Workers Compensation
- Insurtech
- Billing

## Timestamps

- **Created:** 2026-07-25
- **Modified:** 2026-07-25

## APIs

### Origami Risk Authentication API

Token issuance and session verification for the Origami Risk platform APIs. Two documented token formats — a simple JSON payload (Account, User, Password, ClientName) and an OAuth-style client_credentials request combining Client_ID and Client_Secret — plus HMAC authorization for per-call signing, an availability ping, and a token-expiry check.

- **Human URL:** [https://developers.origamirisk.com/reference/authentication-methods](https://developers.origamirisk.com/reference/authentication-methods)
- **Base URL:** `https://{environment}.origamirisk.com/OrigamiApi-v2`
- **OpenAPI:** [openapi/origami-risk-authentication-openapi.json](openapi/origami-risk-authentication-openapi.json)

### Origami Risk Public API

The core Origami Risk platform API — generic domain and entity CRUD (get, upsert, bulk insert, bulk upsert, delete), domain metadata and data dictionary lookups, screen configuration, notes, emails, files and external storage upload, record linking, mail merge, and query/filter syntax validation across every configurable Origami domain.

- **Human URL:** [https://developers.origamirisk.com/reference/domains-entities](https://developers.origamirisk.com/reference/domains-entities)
- **Base URL:** `https://{environment}.origamirisk.com/OrigamiApi`
- **OpenAPI:** [openapi/origami-risk-public-api-openapi.json](openapi/origami-risk-public-api-openapi.json)

### Origami Risk Standard Rating API

Standalone standard rating service that accepts a rating request referencing a rater and intake payloads and returns rating results, offered in both synchronous and asynchronous modes with request retrieval and cancellation. The only one of Origami's four published OpenAPI definitions that carries a complete set of request/response schemas.

- **Human URL:** [https://developers.origamirisk.com/reference](https://developers.origamirisk.com/reference)
- **OpenAPI:** [openapi/origami-risk-standard-rating-api-openapi.json](openapi/origami-risk-standard-rating-api-openapi.json), [openapi/origami-risk-rating-api-openapi.json](openapi/origami-risk-rating-api-openapi.json)

### Origami Risk Quotes and Proposals API

Quote-side policy lifecycle — create and patch proposals, add and remove policy lines, coverages, schedules and linked schedules, list insurance programs, carriers, policy lines and states, run or queue rating and check rating status, run and waive validations, retrieve billing and endorsement billing options, then bind the quote synchronously or by queue and poll bind status.

- **Human URL:** [https://developers.origamirisk.com/reference/quotes-and-proposals](https://developers.origamirisk.com/reference/quotes-and-proposals)
- **Base URL:** `https://{environment}.origamirisk.com/OrigamiApi`

### Origami Risk Policies API

Issue-side policy administration — accept, reject, undo-accept, undo-reject and undo-binding a bound proposal to issue a policy, then endorse, cancel, reinstate, change billing frequency and take payment against the issued policy, with cancellation and reinstatement reason code lookups.

- **Human URL:** [https://developers.origamirisk.com/reference/policies](https://developers.origamirisk.com/reference/policies)
- **Base URL:** `https://{environment}.origamirisk.com/OrigamiApi`

### Origami Risk Billing and Payments API

Billing account operations including making and reversing payments, plus an online policy payment surface integrated with the One Inc payment gateway covering payment submission, payment-method acknowledgement, autopay management feedback and payment feedback callbacks.

- **Human URL:** [https://developers.origamirisk.com/reference/billing-accounts](https://developers.origamirisk.com/reference/billing-accounts)
- **Base URL:** `https://{environment}.origamirisk.com/OrigamiApi`

### Origami Risk Actions API

Real-time and queued platform actions triggered against any domain record — including creating a claim from an incident (the FNOL path), FirstReport, Reserve, Review, RootCause, EDIReport state reporting, AuditResponse, Abstract, AddressValid, AssignSurvey, CorrAction, DataUpdate, DRFScoring, Email, Fax, SMS, MailMerge, Note, Report, RoundRobin, SendMobileForm, StateForm, Task and User actions.

- **Human URL:** [https://developers.origamirisk.com/reference/actions](https://developers.origamirisk.com/reference/actions)
- **Base URL:** `https://{environment}.origamirisk.com/OrigamiApi`

### Origami Risk Webhooks API

Outbound event delivery for the Origami Risk platform — list the configured webhook handlers, retrieve a sample payload for a named handler, and post to a named webhook. A separate One Inc webhook surface handles payment gateway callbacks. No AsyncAPI document or standalone event catalog is published.

- **Human URL:** [https://developers.origamirisk.com/reference/webhooks](https://developers.origamirisk.com/reference/webhooks)
- **Base URL:** `https://{environment}.origamirisk.com/OrigamiApi`

### Origami Risk Reports API

Reporting surface for requesting a report run, retrieving report details and options, validating a report filter, and converting between the platform's view-filter string form and its JSON tree form.

- **Human URL:** [https://developers.origamirisk.com/reference/reports](https://developers.origamirisk.com/reference/reports)
- **Base URL:** `https://{environment}.origamirisk.com/OrigamiApi`

## ACORD Posture

**No ACORD reference found; only a state EDI report action (EDIReport).**

Neither the marketing site nor the developer portal mentions ACORD, AL3, ACORD XML, NGDS, IVANS, agency download, Applied Epic, Vertafore or AMS360. The only standards-shaped artifact in the API surface is a queued action named `EDIReport` (`POST /api/v2/actions/queue/EDIReport/{domain}/{id}`), consistent with US state workers' compensation EDI reporting rather than ACORD transport. Integration marketing names REST API, web services, pre-built integrations and Databricks Delta Sharing instead.

## Common

- [Website](https://www.origamirisk.com/)
- [Developer Portal](https://developers.origamirisk.com/)
- [Documentation](https://developers.origamirisk.com/docs)
- [API Reference](https://developers.origamirisk.com/reference)
- [Getting Started](https://developers.origamirisk.com/docs/getting-started)
- [Authentication](https://developers.origamirisk.com/reference/authentication-methods)
- [Rate Limits](https://developers.origamirisk.com/reference/limits)
- [Product Overview](https://www.origamirisk.com/platform/api-access/)
- [Partners](https://www.origamirisk.com/partners/)

## Maintainers

- Kin Lane — kin@apievangelist.com
