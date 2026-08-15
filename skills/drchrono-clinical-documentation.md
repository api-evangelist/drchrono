---
name: drchrono-clinical-documentation
description: Document a DrChrono visit — record a problem, a medication and a clinical note against the right patient and appointment, respecting the clinical-note lock lifecycle that governs whether a note can still be edited.
api: drchrono
version: v4 (Hunt Valley)
base_url: https://app.drchrono.com
operations:
  - patients_read
  - appointments_read
  - problems_list
  - problems_create
  - medications_list
  - medications_create
  - clinical_notes_list
  - clinical_notes_read
  - allergies_list
  - allergies_create
scopes:
  - clinical:read
  - clinical:write
  - patients:read
  - calendar:read
generated: '2026-08-14'
method: generated
source: openapi/_original/drchrono-rest-api-openapi-schema.json + https://app.drchrono.com/api-docs/
---

# Document a DrChrono clinical encounter

Everything in this flow is PHI and most of it is a Level 2 endpoint in DrChrono's own API terms
(`/api/clinical_notes`, `/api/lab_orders`, `/api/line_items`). Treat writes as consequential.

## Before you start

- OAuth 2.0 token with `clinical:write`, plus the in-app clinical permission on the authorizing user.
- Confirm the patient and the appointment first. Every clinical object hangs off `patient`, and most
  hang off `doctor` and often `appointment`.

## Steps

1. **Anchor the encounter.**
   `GET /api/patients/{id}` (`patients_read`) and `GET /api/appointments/{id}` (`appointments_read`).
   Carry those integer ids into every write below — they are the join keys of the whole clinical
   graph (see `data-model/drchrono-data-model.yml`).

2. **Record the problem.**
   `GET /api/problems` (`problems_list`) filtered by `patient` to avoid duplicating an existing
   active problem, then `POST /api/problems` (`problems_create`) with `patient`, `doctor` and the
   coded diagnosis. Expect `201`.

3. **Record the medication.**
   `GET /api/medications` (`medications_list`) then `POST /api/medications` (`medications_create`).
   The `ndc` field is available (added 2019-12-02) and should be populated where you have it.

4. **Record allergies** if the intake produced any: `GET /api/allergies` (`allergies_list`), then
   `POST /api/allergies` (`allergies_create`).

5. **Read the clinical note.**
   `GET /api/clinical_notes` (`clinical_notes_list`) filtered by `patient` and `appointment`, or
   `GET /api/clinical_notes/{id}` (`clinical_notes_read`).

## The lock lifecycle matters

A DrChrono clinical note can be **locked**. A locked note is the signed legal record of the encounter.
Do not build a flow that assumes a note stays writable: subscribe to the `CLINICAL_NOTE_LOCK` and
`CLINICAL_NOTE_UNLOCK` webhook events and treat a lock as the terminal state for automated edits.
Structured clinical data (problems, medications, allergies) remains addressable after the note locks;
the note body does not.

## Verbose responses

Several clinical endpoints omit expensive fields unless you pass `verbose=true`. Doing so drops both
the default and maximum page size to **50**, so a verbose sweep costs roughly five times the calls of
a non-verbose one against a 500-per-hour budget. Ask for verbose only on the specific records you
need it for.

## Failure handling

| Status | Do |
|---|---|
| 400 | Field-keyed JSON body names the offending field. Do not retry unchanged. |
| 401 | Refresh the 48-hour token. |
| 403 | `clinical:write` not granted, or the in-app clinical permission is not set. Not retryable. |
| 404 | Patient or appointment is outside this token's practice group. |
| 409 | Conflicting write — re-read before retrying. |
| 429 | Back off to the top of the next hour. |

## Do not

- Do not retry a failed `POST` blind. There is no idempotency key, and a duplicated problem or
  medication on a chart is a clinical-safety issue.
- Do not write clinical data you cannot attribute to a doctor — `doctor` is the accountability field
  on these objects and DrChrono's audit log (`/api/audit_log`) records against it.
