---
name: drchrono-appointment-scheduling
description: Book a DrChrono appointment safely — read availability and the appointment profile, create the appointment, then read it back to confirm, working within the 20-record page cap that only this endpoint carries.
api: drchrono
version: v4 (Hunt Valley)
base_url: https://app.drchrono.com
operations:
  - availability
  - offices_list
  - doctors_list
  - appointment_profiles_list
  - appointments_list
  - appointments_create
  - appointments_read
  - appointments_partial_update
scopes:
  - calendar:read
  - calendar:write
  - patients:summary:read
  - user:read
generated: '2026-08-14'
method: generated
source: openapi/_original/drchrono-rest-api-openapi-schema.json + https://app.drchrono.com/api-docs/
---

# Schedule a DrChrono appointment

## Before you start

- OAuth 2.0 authorization code token, 48-hour lifetime, refreshed via `refresh_token`.
- `calendar:write` **plus** the matching in-app permission. Scope without permission returns `403`.
- `/api/appointments` is the one endpoint whose page size is capped at **20**, not 250. Paginate.

## Steps

1. **Resolve the context.**
   `GET /api/doctors` (`doctors_list`) and `GET /api/offices` (`offices_list`) to get the integer
   `doctor` and `office` ids the appointment will reference. Cache these — they change rarely and
   your hourly budget is 500 calls.

2. **Check availability.**
   `GET /api/availability` (`availability`) for the doctor and date window you want.

3. **Read the appointment profile** if the practice uses them.
   `GET /api/appointment_profiles` (`appointment_profiles_list`) supplies default duration, reason
   and colour so the booking matches how the practice actually runs its schedule.

4. **Check for a clash.**
   `GET /api/appointments` (`appointments_list`) filtered by `doctor` and date. Remember the 20-record
   cap and follow `next` until exhausted before concluding the slot is free.

5. **Create the appointment.**
   `POST /api/appointments` (`appointments_create`) with `patient`, `doctor`, `office`,
   `scheduled_time` and `duration`. Expect `201`.

6. **Read it back.**
   `GET /api/appointments/{id}` (`appointments_read`) and confirm `scheduled_time`, `duration` and
   `status`. This is not ceremony — step 5 is not idempotent, so the read-back is how you distinguish
   "created once" from "created twice" after a timeout.

7. **To change it**, `PATCH /api/appointments/{id}` (`appointments_partial_update`). Expect `204` and
   an empty body.

## Timestamps

`scheduled_time` is an ISO 8601 timestamp with **no timezone offset** (`2021-02-14T13:40:39`).
It is interpreted in the office's local timezone. Send local wall time for the office you resolved in
step 1 — do not send UTC and do not append `Z`.

## Failure handling

| Status | Do |
|---|---|
| 400 | Read the field-keyed JSON body; fix the named field. |
| 401 | Refresh the 48-hour token, retry once. |
| 403 | Missing `calendar:write` scope or missing in-app permission. Not retryable. |
| 404 | The doctor, office or patient id is outside this token's practice group. |
| 409 | Conflicting write. Re-read the schedule before trying again. |
| 429 | 500/hour or 10/sec exceeded. Back off to the top of the next hour; no `Retry-After` header exists. |

## Events

If you need to know about schedule changes made outside your integration, register a webhook on your
API application for `APPOINTMENT_CREATE`, `APPOINTMENT_MODIFY` and `APPOINTMENT_DELETE` rather than
polling — polling `/api/appointments` at 20 records per page will exhaust a 500-call hour quickly.
See `asyncapi/drchrono-webhooks-asyncapi.yml` for headers, HMAC-SHA256 verification and the
1h/3h/7h retry schedule.
