---
name: Issue and service a policy on Origami Risk
description: >-
  Accept a bound proposal to issue a policy, then endorse, cancel, reinstate, change
  billing frequency and take payment against the issued policy — including the undo
  paths Origami exposes for each state transition.
api: https://developers.origamirisk.com/reference/policies
generated: '2026-07-25'
method: generated
source: https://developers.origamirisk.com/reference/policies
operations:
  - POST /api/Policies/{proposalID}/Accept
  - POST /api/Policies/{proposalID}/UndoAccept
  - POST /api/Policies/{proposalID}/Reject
  - POST /api/Policies/{proposalID}/UndoReject
  - POST /api/Policies/{proposalID}/UndoBinding
  - POST /api/Policies/{policyId}/Endorse
  - POST /api/Policies/{policyId}/Cancel
  - POST /api/Policies/Codes/CancellationReasonCodes
  - POST /api/Policies/{policyId}/Reinstate
  - POST /api/Policies/Codes/ReinstatementReasonCodes
  - POST /api/Policies/{policyId}/ChangeBillingFrequency
  - POST /api/Policies/{policyId}/MakePayment
  - POST /api/BillingAccounts/{billingAccountId}/MakePayment
  - POST /api/BillingAccounts/{billingAccountId}/ReversePayment
  - POST /api/Quotes/Proposals/{proposalId}/CreateMember
---

# Issue and service a policy on Origami Risk

All paths below come from the public Origami Risk reference. The published OpenAPI
definitions do not cover the policy surface, so operations are named by method and path.

## Prerequisites

- A bound proposal (see `skills/origami-risk-quote-to-bind.md`).
- A token in the `Token` header, against the correct `{environment}` host.

## Issue

1. **Accept the proposal.** `POST /api/Policies/{proposalID}/Accept`, optional
   `comments` query parameter which is written to the workflow log. Documented behaviour:
   - Loads the proposal and validates it can be accepted (permission, status, business
     rules, validations).
   - Checks that any online payment has not been refunded.
   - Sets status to `Accepted`, or `Accepted with Changes` if the proposal was modified
     since it was quoted.
   - Generates or reuses a policy number for renewals; generates one for other types when
     the PolicySet is configured to do so on accept.
   - Logs the action and fires a status-change event inside Origami's workflow engine.
   - **It does not set `billingFrequency`, `installmentFees`, `totalCost` or
     `securityDeposit`.** Set billing details in a separate call before or after accept.
2. **Reject instead** with `POST /api/Policies/{proposalID}/Reject`.
3. **Undo paths exist for every transition** — `UndoAccept`, `UndoReject`, `UndoBinding`.
   Prefer the matching undo over re-running a forward transition.
4. **Create the policy holder record** if the workflow calls for it:
   `POST /api/Quotes/Proposals/{proposalId}/CreateMember`.

## Service the issued policy

- **Endorse:** `POST /api/Policies/{policyId}/Endorse` creates the endorsement
  transaction; rate and bind it through the quote flow when it is a rated change.
- **Cancel:** fetch valid reasons with
  `POST /api/Policies/Codes/CancellationReasonCodes`, then
  `POST /api/Policies/{policyId}/Cancel`.
- **Reinstate:** fetch reasons with
  `POST /api/Policies/Codes/ReinstatementReasonCodes`, then
  `POST /api/Policies/{policyId}/Reinstate`.
- **Change billing frequency:** `POST /api/Policies/{policyId}/ChangeBillingFrequency`.
- **Take payment:** `POST /api/Policies/{policyId}/MakePayment` creates and allocates a
  payment transaction. Billing-account-level equivalents are
  `POST /api/BillingAccounts/{billingAccountId}/MakePayment` and
  `POST /api/BillingAccounts/{billingAccountId}/ReversePayment`.

## Rules an agent must follow

- **Money and contract state, with no idempotency key.** Accept, cancel, reinstate and
  every payment call are consequential and not retry-safe. Re-read the policy or billing
  account before repeating a write, and use the published undo operations rather than
  compensating by hand.
- **Look up reason codes; never guess them.** The cancellation and reinstatement code
  lists are per-tenant and are served by their own endpoints.
- Inspect response bodies — the platform returns validation problems inside 200
  responses on parts of this surface.
- Online policy payments taken through the One Inc gateway are acknowledged by inbound
  callbacks (`OneIncAcknowledgePaymentMethod`, `OneIncPaymentFeedback`,
  `OneIncManageAutoPayFeedback`) that One Inc calls into Origami. Do not expect Origami
  to call you — see `asyncapi/origami-risk-webhooks.yml`.

## Related artifacts

- `conventions/origami-risk-conventions.yml`
- `errors/origami-risk-problem-types.yml`
- `data-model/origami-risk-data-model.yml`
