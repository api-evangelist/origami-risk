# Origami Risk

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
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Origami Risk is a Chicago-headquartered risk, safety and insurance SaaS company founded in 2009 that began as a single-version cloud RMIS (risk management information system) and grew into a core-systems platform for the United States property and casualty market. It sells policy administration, digital underwriting, rating, billing, premium audit, claims administration, compliance and EHS/GRC modules to carriers, MGAs, program administrators, third-party administrators, risk pools, brokers, healthcare systems and large self-insureds, across workers' compensation, medical professional liability, personal auto and homeowners lines. Unlike most US insurance organizations, Origami Risk sits in the software layer between carriers and distribution and therefore does publish a genuinely public, self-serve developer portal at developers.origamirisk.com — a ReadMe-hosted reference readable without login, covering quote and proposal creation, rating, bind, policy issue/endorse/cancel, billing and payments, claims-from-incident and first-report actions, files, reports, domain metadata and outbound webhooks. Four OpenAPI definitions are downloadable from the portal's spec registry, though three of the four are thin scaffolds and the bulk of the reference is hand-authored per-endpoint documentation rather than a complete machine-readable spec. Access to a live tenant is still commercial — the base URL is a per-customer https://{environment}.origamirisk.com host and authentication is token-based or HMAC against a provisioned account — and no ACORD, AL3, IVANS or NGDS conformance is claimed anywhere in the marketing site or the developer portal.

**Areas:** Insurance · United States · Property and Casualty · Policy Administration · Claims · Underwriting · Core Systems · Risk Management · Workers Compensation · Insurtech · Billing

## Origami Risk on APIs.io

Everything profiled for Origami Risk is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/origami-risk/](https://apis.io/providers/origami-risk/)** — or go straight to one of the APIs or properties below.

### APIs (11)

- [Origami Risk Quotes and Proposals API](https://apis.io/apis/origami-risk/origami-risk-quotes-and-proposals-api/) — Quote-side policy lifecycle — create and patch proposals, add and remove policy lines, coverages, schedules and linked schedules, list insurance programs,…
- [Origami Risk Policies API](https://apis.io/apis/origami-risk/origami-risk-policies-api/) — Issue-side policy administration — accept, reject, undo-accept, undo-reject and undo-binding a bound proposal to issue a policy, then endorse, cancel,…
- [Origami Risk Billing and Payments API](https://apis.io/apis/origami-risk/origami-risk-billing-accounts-api/) — Billing account operations including making and reversing payments, plus an online policy payment surface integrated with the One Inc payment gateway…
- [Origami Risk Actions API](https://apis.io/apis/origami-risk/origami-risk-actions-api/) — Real-time and queued platform actions triggered against any domain record — including creating a claim from an incident (the FNOL path), FirstReport,…
- [Origami Risk Webhooks API](https://apis.io/apis/origami-risk/origami-risk-webhooks-api/) — Inbound webhook surface for the Origami Risk platform. An Origami "webhook handler" is a bespoke endpoint built inside Origami by the services team that an…
- [Origami Risk Reports API](https://apis.io/apis/origami-risk/origami-risk-reports-api/) — Reporting surface for requesting a report run, retrieving report details and options, validating a report filter, and converting between the platform's…
- [Origami Risk Authentication API](https://apis.io/apis/origami-risk/origami-risk-authentication-api/) — The Authentication API from Origami Risk — 4 operation(s) for authentication.
- [Origami Risk Has Token Expired API](https://apis.io/apis/origami-risk/origami-risk-hastokenexpired-api/) — The HasTokenExpired API from Origami Risk — 1 operation(s) for hastokenexpired.
- [Origami Risk New Endpoint 1 API](https://apis.io/apis/origami-risk/origami-risk-new-endpoint-1-api/) — The New Endpoint 1 API from Origami Risk — 1 operation(s) for new endpoint 1.
- [Origami Risk New Endpoint API](https://apis.io/apis/origami-risk/origami-risk-new-endpoint-api/) — The New Endpoint API from Origami Risk — 1 operation(s) for new endpoint.
- [Origami Risk Requests API](https://apis.io/apis/origami-risk/origami-risk-requests-api/) — The Requests API from Origami Risk — 3 operation(s) for requests.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Origami Risk Webhooks](https://apis.io/asyncapis/origami-risk/origami-risk-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [origami-risk](https://apis.io/servers/origami-risk/origami-risk/)

#### Collections (4)

Postman and Open Collection request collections.

- [Origami Risk Authentication](https://apis.io/collections/origami-risk/open-origami-risk-authentication/)
- [Origami Risk Public API](https://apis.io/collections/origami-risk/open-origami-risk-public-api/)
- [Rating API](https://apis.io/collections/origami-risk/open-origami-risk-rating-api/)
- [Rating API](https://apis.io/collections/origami-risk/open-origami-risk-standard-rating-api/)

#### Overlays (4)

OpenAPI overlays, including conversational phrasing for each operation.

- [Origami Risk Authentication Overlay](https://apis.io/overlays/origami-risk/origami-risk-authentication-overlay/)
- [Origami Risk Public Api Overlay](https://apis.io/overlays/origami-risk/origami-risk-public-api-overlay/)
- [Origami Risk Rating Api Overlay](https://apis.io/overlays/origami-risk/origami-risk-rating-api-overlay/)
- [Origami Risk Standard Rating Api Overlay](https://apis.io/overlays/origami-risk/origami-risk-standard-rating-api-overlay/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Origami Risk Authentication](https://apis.io/security/origami-risk/origami-risk-authentication/)
- [Origami Risk Domain Security](https://apis.io/security/origami-risk/origami-risk-domain-security/)
- [Origami Risk Trust Center](https://apis.io/security/origami-risk/origami-risk-trust-center/)

#### Rate limits (1)

Published request limits.

- [Origami Risk Rate Limits](https://apis.io/rate-limits/origami-risk/origami-risk-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.origamirisk.com/)
- [Documentation](https://developers.origamirisk.com/docs)
- [Getting started](https://developers.origamirisk.com/docs/getting-started)
- [API reference](https://developers.origamirisk.com/reference)
- [Pricing](https://www.origamirisk.com/serviceterms/aipricing/)
- [Support](https://www.origamirisk.com/contact/)
- [Changelog](https://www.origamirisk.com/platform/product-updates/)
- [Blog](https://www.origamirisk.com/resources/insights/)
- [Compliance](https://trust.origamirisk.com/)
- [Terms of service](https://www.origamirisk.com/origami-risk-website-terms-use/)
- [Privacy policy](https://www.origamirisk.com/privacy-policy/)
- [GitHub](https://github.com/origamirisk)
- [Status](https://status.origamirisk.com/)

## API Evangelist rating

**Kin Score 51.0** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 71.4 |
| Contract quality | 48.5 |
| Contract governance | 4.5 |
| Operational transparency | 73.7 |
| Developer ergonomics | 50.6 |
| Access clarity | 53.9 |

**Agent readiness 30.5** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | — |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | distinct |

**Regulatory alignment 26.3** — Insurance

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/origami-risk/](https://apis.io/providers/origami-risk/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Origami Risk: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/origami-risk/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
