---
name: drchrono-fhir-record-retrieval
description: Read a patient's USCDI record from DrChrono's SMART on FHIR R4 server — the ONC-certified, read-only interoperability surface that is a completely separate contract from the REST v4 API, with its own authorization server, scopes and identifiers.
api: drchrono-fhir
version: R4 (4.0.1)
base_url: https://drchrono-fhirpresentation.everhealthsoftware.com/fhir/drchrono/{practice_id}/r4
operations:
  - Patient.search
  - Patient.read
  - Condition.search
  - MedicationRequest.search
  - AllergyIntolerance.search
  - Observation.search
  - DiagnosticReport.search
  - DocumentReference.search
  - Immunization.search
  - Patient.$export
scopes:
  - openid
  - fhirUser
  - offline_access
  - patient/*.read
  - user/*.read
  - system/*.read
generated: '2026-08-14'
method: generated
source: >-
  fhir/drchrono-fhir-r4-capabilitystatement.json,
  well-known/drchrono-fhir-smart-configuration.json,
  https://drchrono-fhirpresentation.everhealthsoftware.com/drchrono/498711/r4/Home/ApiDocumentation
---

# Read a patient record from DrChrono over FHIR

This is **not** the REST v4 API. Different host, different authorization server, different scopes,
different identifiers, and it is **read-only**. Nothing you learned about `/api/patients` transfers.

## Resolve the service base

Every practice has its own FHIR base of the form
`https://drchrono-fhirpresentation.everhealthsoftware.com/fhir/drchrono/{practice_id}/r4`.

The public, unauthenticated service base directory is
`https://drchrono-fhirpresentation.everhealthsoftware.com/fhir/r4/endpoints` — a FHIR `Bundle` of
`Endpoint` and `Organization` resources (210 entries, 105 practices). Resolve the practice there
rather than hard-coding the `498711` used in DrChrono's documentation examples.

## Authorize

- Authorization server: `https://drchrono-fhir.everhealthsoftware.com/core`
- Discovery: `/.well-known/openid-configuration` at that root, and `/.well-known/smart-configuration`
  at the FHIR base — both saved in `well-known/`.
- Flows: `authorization_code` (use **PKCE**, `S256` — it is the only challenge method advertised),
  `client_credentials` for backend services, `refresh_token`, and `urn:ietf:params:oauth:grant-type:device_code`.
- Client auth: `client_secret_basic`, `client_secret_post` or `private_key_jwt`.
- Include `aud=<fhir base>` on the authorize call, as DrChrono's own configuration table shows.

**Registration is not dynamic.** The `registration_endpoint` is a permissions console. DrChrono
states that only an EHR vendor admin can add a client app, supplying application URL, redirect URL,
logout URL, scope list, application name and OAuth flow. Plan for a human step.

## Read

1. `GET [base]/Patient?identifier=...` or `?family=&given=&birthdate=`
   (search params: `patient`, `_id`, `identifier`, `name`, `gender`, `family`, `given`, `birthdate`).
2. `GET [base]/Condition?patient={id}&clinical-status=active`
3. `GET [base]/MedicationRequest?patient={id}&status=active`
4. `GET [base]/AllergyIntolerance?patient={id}`
5. `GET [base]/Observation?patient={id}&category=vital-signs` — and `category=laboratory` for labs,
   `code=72166-2` for smoking status.
6. `GET [base]/DocumentReference?patient={id}&type=...` then `GET [base]/Binary/{id}` for the C-CDA.

All 27 supported resources declare only `read` and `search-type`. There is no `create`, `update`,
`delete`, `history` or `patch` anywhere in the CapabilityStatement. Do not attempt writes.

## Bulk

For a population rather than a patient:

```
GET [base]/Patient/$export          # all patients you are authorized for
GET [base]/Group/{id}/$export       # a defined group
Accept: application/fhir+json
Prefer: respond-async
```

Kick-off returns `202 Accepted` with a `Content-Location` polling URL; poll it, then fetch the
ndjson files. Authorize with `client_credentials` + a `private_key_jwt` client assertion
(`client_assertion_type: urn:ietf:params:oauth:client-assertion-type:jwt-bearer`) and scope
`system/*.read`.

## Identifiers do not cross over

A FHIR `Patient.id` is an opaque string on this server. A REST v4 patient id is an integer on
`app.drchrono.com`. They are different identifier spaces. If you need to join the two surfaces, match
on `identifier`, name and birth date — never assume the ids correspond.

## Scope of the data

DrChrono states the FHIR API serves read-only requests for patient health information **that is part
of the USCDI**, under 45 CFR §170.315(g)(7), (g)(9) and (g)(10), and that C-CDA filtering uses the
latest cumulative C-CDA per patient by `EffectiveDateTime`. Anything outside USCDI — billing, tasks,
scheduling, messages — is on the REST v4 API only.

## Errors

Failures return a FHIR `OperationOutcome` (`application/fhir+json`) with `issue[].severity`,
`issue[].code` and `issue[].diagnostics`. This is a different error shape from the REST API's
field-keyed JSON object; do not share a parser between the two.
