---
name: drchrono-patient-registration
description: Find an existing DrChrono patient before creating one, then create or update the record — the upsert every DrChrono integration needs first, and the one most likely to create duplicate charts if done wrong.
api: drchrono
version: v4 (Hunt Valley)
base_url: https://app.drchrono.com
operations:
  - patients_summary_list
  - patients_list
  - patients_create
  - patients_partial_update
  - patients_read
scopes:
  - patients:summary:read
  - patients:summary:write
  - patients:read
  - patients:write
generated: '2026-08-14'
method: generated
source: openapi/_original/drchrono-rest-api-openapi-schema.json + https://app.drchrono.com/api-docs/
---

# Register or update a DrChrono patient

DrChrono has no idempotency key. If you POST a patient twice you get two charts, and in an EHR a
duplicate chart is a clinical-safety problem, not a data-hygiene one. Always search first.

## Before you start

- Get an access token via OAuth 2.0 authorization code against
  `https://app.drchrono.com/o/authorize/` and `https://app.drchrono.com/o/token/`. Tokens last
  **48 hours**; refresh with the `refresh_token` grant.
- A scope alone is not enough. DrChrono gates every endpoint on **both** the OAuth scope granted at
  authorization **and** an in-app permission a primary user sets in the DrChrono web app. A correctly
  scoped token still returns `403` when the permission is missing.
- Budget your calls: **500 requests per hour** per API application, resetting at the top of the hour,
  plus a **10 requests/second** burst throttle. There are no rate-limit response headers — count your
  own calls.

## Steps

1. **Search for the patient by name.**
   `GET /api/patients_summary` (`patients_summary_list`) with `first_name` and `last_name`.
   Use the summary endpoint rather than the full one where you only need identity — it needs only
   `patients:summary:read` and returns less PHI, which is the right minimum-necessary posture under
   the HIPAA obligations in DrChrono's API terms.
   Read `results` (the reference documents `previous`/`results`/`next`; the published OpenAPI declares
   `previous`/`data`/`next` for the same shape — tolerate both).

2. **Disambiguate.** Match on last name + date of birth + gender, not on name alone. DrChrono's own
   duplicate check uses exactly first name, last name, date of birth and gender.

3. **If a match exists, update it.**
   `PATCH /api/patients/{id}` (`patients_partial_update`) with only the fields that changed.
   Expect `204 No Content` and an empty body — re-read with `GET /api/patients/{id}`
   (`patients_read`) if you need the resulting object.
   Never use `PUT` (`patients_update`) unless you intend to replace the entire record; DrChrono's
   `PUT` is a full replace.

4. **If no match exists, create it.**
   `POST /api/patients` (`patients_create`). Expect `201 Created` with the created object.
   - `chart_id`, if you set it, must match the format `AAAA000000` (changed 2022-06-23).
   - If the practice has the `api_prevent_patient_duplicate` feature flag enabled, a create that
     collides on first name / last name / date of birth / gender returns **409** with
     "Potential duplicate patient(s) found.". Treat that as a signal to go back to step 1, not as an
     error to retry. Only send `allow_duplicates: true` when a human has confirmed the two people are
     genuinely different.
   - Set pharmacies with `preferred_pharmacies` (an array). `default_pharmacy` still exists but
     DrChrono has announced it will be deprecated in favour of `preferred_pharmacies`.

## Failure handling

| Status | Meaning | Do |
|---|---|---|
| 400 | Validation failure. Body is a JSON object keyed by field name with a list of messages. | Fix the named field. Do not retry unchanged. |
| 401 | Token missing, expired (48h) or revoked. | Refresh and retry once. |
| 403 | Scope not granted, or the in-app permission is not set for this user. | Re-authorize with the scope; ask the practice admin to grant the permission. Retrying will not help. |
| 409 | Duplicate patient guard, or conflicting write. | Return to step 1. Never blind-retry. |
| 429 | Hourly limit (500) or 10/sec burst exceeded. | Back off to the top of the next hour. There is no `Retry-After`. |
| 302 | Some endpoints redirect. | Replay the **original method and headers** against `Location`; most HTTP clients get this wrong. |

## Do not

- Do not retry a `POST` on timeout — there is no idempotency key, and you will create a second chart.
- Do not request all scopes. Omitting the `scope` parameter on the authorize call requests
  **every** scope; ask for the four listed above.
- Do not assume the FHIR surface can help here. DrChrono's SMART on FHIR R4 server is **read-only**
  and its resource ids do not map to these integer patient ids.
