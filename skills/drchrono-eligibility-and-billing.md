---
name: drchrono-eligibility-and-billing
description: Run the DrChrono revenue cycle from the API — check insurance eligibility, review the day sheet, inspect line items and add a claim billing note — all on Level 2 endpoints that touch money and PHI together.
api: drchrono
version: v4 (Hunt Valley)
base_url: https://app.drchrono.com
operations:
  - insurances_list
  - eligibility_checks_list
  - eligibility_checks_read
  - line_items_list
  - line_items_read
  - claim_billing_notes_list
  - claim_billing_notes_create
  - patient_payments_list
  - patient_payments_create
  - transactions_list
scopes:
  - billing:read
  - billing:write
  - billing:patient-payment:read
  - billing:patient-payment:write
  - patients:read
generated: '2026-08-14'
method: generated
source: openapi/_original/drchrono-rest-api-openapi-schema.json + https://app.drchrono.com/api-docs/
---

# Work a DrChrono claim

Every endpoint in this skill is classified **Level 2** in DrChrono's published API terms, and the
patient-payment endpoints sit behind their own dedicated scopes
(`billing:patient-payment:read` / `billing:patient-payment:write`) separate from general
`billing:read` / `billing:write`. Request the narrower pair when you only touch payments.

## Steps

1. **Read the coverage.**
   `GET /api/insurances` (`insurances_list`) filtered by `patient` to get the payer, member id and
   plan on file.

2. **Check eligibility.**
   `GET /api/eligibility_checks` (`eligibility_checks_list`) and
   `GET /api/eligibility_checks/{id}` (`eligibility_checks_read`) for the result detail. Eligibility
   checks are unlimited on DrChrono's Advanced tier and above — they are a plan feature, not an API
   quota, so the constraint on sweeping them is your 500-calls-per-hour API budget, not entitlement.

3. **Review the charges.**
   `GET /api/line_items` (`line_items_list`) filtered by `appointment` or date, then
   `GET /api/line_items/{id}` (`line_items_read`) for a single charge. Line items are where CPT/HCPCS
   codes, units and allowed amounts live.

4. **Read the money movement.**
   `GET /api/transactions` (`transactions_list`) for adjudication and posting activity,
   `GET /api/patient_payments` (`patient_payments_list`) for what the patient has paid.

5. **Annotate the claim.**
   `POST /api/claim_billing_notes` (`claim_billing_notes_create`) to attach a note to the claim.
   Expect `201`. This is the safe write in the flow — it records intent without moving money.

6. **Post a patient payment** only when your integration is the system of record for that payment:
   `POST /api/patient_payments` (`patient_payments_create`) with the
   `billing:patient-payment:write` scope. Expect `201`.

## Decimals

DrChrono's `decimal` type is a **string** truncated to two decimal places. It accepts an integer,
float or string on input but **always returns a string**. Parse it as a decimal, never as a float,
and never round-trip it through a binary float before sending it back.

## The retry hazard on money

There is no idempotency key on this API. `POST /api/patient_payments` retried after a timeout posts
the payment twice. Before any retry, re-read `patient_payments_list` for the patient and date and
confirm the payment is absent. Record your own client-side request id in your ledger; DrChrono
publishes no request-id header to correlate against.

## Events

Register `LINE_ITEM_CREATE`, `LINE_ITEM_MODIFY`, `LINE_ITEM_DELETE`,
`LINE_ITEM_TRANSACTION_DELETE` and `CASH_PAYMENT_DELETE` webhooks rather than polling the ledger.
Deletions in particular are only visible as events — a polled list simply stops showing the row.

## Failure handling

| Status | Do |
|---|---|
| 400 | Field-keyed JSON body. Fix and resend. |
| 401 | Refresh the 48-hour token. |
| 403 | Billing scope not granted, or the practice has not granted the in-app billing permission. |
| 409 | Conflicting write — re-read the claim state first. |
| 429 | Back off to the top of the next hour; no `Retry-After`. |

DrChrono publishes no decline-code or denial-reason registry through the API. Denial detail arrives
in the EOB and transaction records (`/api/eobs`, `/api/transactions`), not as a coded error catalog.
