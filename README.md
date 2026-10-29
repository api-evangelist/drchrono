# drchrono

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

drchrono, part of EverCommerce's EverHealth portfolio, is an all-in-one EHR, practice management and medical billing platform for independent US medical practices. It publishes two distinct machine-readable API surfaces. The proprietary REST v4 API (codename Hunt Valley) is an OAuth 2.0-secured contract of 170 paths and 329 operations covering patients, scheduling, clinical documentation, e-prescribing, labs, tasks, messaging and the full revenue cycle, published as a live OpenAPI 3.0 at https://app.drchrono.com/openapi-schema and complemented by a 27-event webhook surface. Alongside it drchrono runs a read-only, ONC-certified SMART on FHIR R4 server exposing 27 US Core resource types for USCDI interoperability, with SMART App Launch and OpenID Connect discovery, FHIR Bulk Data Export, and a public service base directory covering 105 practices. The two estates share no tokens, no scopes and no identifier space.

**Areas:** Electronic Health Records · Healthcare · Medical Records · Practice Management · HIPAA · Appointments · Billing · Prescriptions · Lab Integration · FHIR · SMART on FHIR · USCDI

## drchrono on APIs.io

Everything profiled for drchrono is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/drchrono/](https://apis.io/providers/drchrono/)** — or go straight to one of the APIs or properties below.

### APIs (7)

- [drchrono Administrative API](https://apis.io/apis/drchrono/drchrono-administrative-api/) — Create and manage administrative resources
- [drchrono Audit API](https://apis.io/apis/drchrono/drchrono-audit-api/) — Search Audit Logs
- [drchrono Availability API](https://apis.io/apis/drchrono/drchrono-availability-api/) — The Availability API from drchrono — 1 operation(s) for availability.
- [drchrono Billing API](https://apis.io/apis/drchrono/drchrono-billing-api/) — Create and manage billing resources
- [drchrono Clinical API](https://apis.io/apis/drchrono/drchrono-clinical-api/) — Create and manage clinical resources
- [drchrono Practice Management API](https://apis.io/apis/drchrono/drchrono-practice-management-api/) — Create and manage practice management resources
- [drchrono SMART on FHIR R4 API](https://apis.io/apis/drchrono/drchrono-fhir-r4-api/) — The ONC-certified SMART on FHIR R4 interoperability API for the DrChrono EHR. Read-only, 27 US Core resource types, FHIR 4.0.1, with a live…

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### Arazzo workflows (5)

Multi-step workflows that chain operations into a business task.

- [drChrono Appointment Scheduling](https://apis.io/arazzos/drchrono/drchrono-appointment-scheduling-workflow/)
- [drChrono Chart Export](https://apis.io/arazzos/drchrono/drchrono-chart-export-workflow/)
- [drChrono Clinical Documentation](https://apis.io/arazzos/drchrono/drchrono-clinical-documentation-workflow/)
- [drChrono Eligibility and Billing](https://apis.io/arazzos/drchrono/drchrono-eligibility-and-billing-workflow/)
- [drChrono Patient Registration (Upsert)](https://apis.io/arazzos/drchrono/drchrono-patient-registration-workflow/)

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [DrChrono EHR Webhooks](https://apis.io/asyncapis/drchrono/drchrono-webhooks-asyncapi/)

#### Event channels (1)

Individual events and webhooks the provider emits.

- [{callbackUrl}](https://apis.io/channels/drchrono/callbackurl/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Drchrono Agentic Access](https://apis.io/agentic-access/drchrono/drchrono-agentic-access/)

#### JSON Schema (100)

The data models the API reads and writes.

- [Appointment](https://apis.io/schemas/drchrono/appointment/)
- [AppointmentProfile](https://apis.io/schemas/drchrono/appointmentprofile/)
- [AppointmentTemplate](https://apis.io/schemas/drchrono/appointmenttemplate/)
- [AsyncResourceError](https://apis.io/schemas/drchrono/asyncresourceerror/)
- [AuditLog](https://apis.io/schemas/drchrono/auditlog/)
- [BillingLineItem](https://apis.io/schemas/drchrono/billinglineitem/)
- [BillingLineItemDeletion](https://apis.io/schemas/drchrono/billinglineitemdeletion/)
- [BillingProfile](https://apis.io/schemas/drchrono/billingprofile/)
- [CarePlan](https://apis.io/schemas/drchrono/careplan/)
- [CareTeamMember](https://apis.io/schemas/drchrono/careteammember/)
- [CashPayment](https://apis.io/schemas/drchrono/cashpayment/)
- [CashPaymentLog](https://apis.io/schemas/drchrono/cashpaymentlog/)
- [ClaimBillingNotes](https://apis.io/schemas/drchrono/claimbillingnotes/)
- [ClinicalNote](https://apis.io/schemas/drchrono/clinicalnote/)
- [ConsentForm](https://apis.io/schemas/drchrono/consentform/)
- [Coverage](https://apis.io/schemas/drchrono/coverage/)
- [CustomAppointmentFieldType](https://apis.io/schemas/drchrono/customappointmentfieldtype/)
- [CustomInsurancePlanName](https://apis.io/schemas/drchrono/custominsuranceplanname/)
- [CustomPatientFieldType](https://apis.io/schemas/drchrono/custompatientfieldtype/)
- [CustomVitalType](https://apis.io/schemas/drchrono/customvitaltype/)
- [Doctor](https://apis.io/schemas/drchrono/doctor/)
- [DoctorFeeSchedule](https://apis.io/schemas/drchrono/doctorfeeschedule/)
- [DoctorMessage](https://apis.io/schemas/drchrono/doctormessage/)
- [DoctorOptions](https://apis.io/schemas/drchrono/doctoroptions/)
- [Appointment](https://apis.io/schemas/drchrono/drchrono-appointment/)
- …and 75 more published on APIs.io

#### JSON-LD (1)

Linked-data contexts for the provider's vocabulary.

- [Drchrono Context](https://apis.io/jsonld/drchrono/drchrono-context/)

#### Vocabulary (1)

The provider's domain terms, defined.

- [drchrono API Vocabulary](https://apis.io/vocabularies/drchrono/drchrono-vocabulary/)

#### Examples (69)

Example requests and responses.

- [Appointment Example](https://apis.io/examples/drchrono/appointment-example/)
- [Appointmentprofile Example](https://apis.io/examples/drchrono/appointmentprofile-example/)
- [Appointmenttemplate Example](https://apis.io/examples/drchrono/appointmenttemplate-example/)
- [Billinglineitem Example](https://apis.io/examples/drchrono/billinglineitem-example/)
- [Billingprofile Example](https://apis.io/examples/drchrono/billingprofile-example/)
- [Careplan Example](https://apis.io/examples/drchrono/careplan-example/)
- [Careteammember Example](https://apis.io/examples/drchrono/careteammember-example/)
- [Cashpayment Example](https://apis.io/examples/drchrono/cashpayment-example/)
- [Cashpaymentlog Example](https://apis.io/examples/drchrono/cashpaymentlog-example/)
- [Claimbillingnotes Example](https://apis.io/examples/drchrono/claimbillingnotes-example/)
- [Clinicalnote Example](https://apis.io/examples/drchrono/clinicalnote-example/)
- [Consentform Example](https://apis.io/examples/drchrono/consentform-example/)
- [Coverage Example](https://apis.io/examples/drchrono/coverage-example/)
- [Customappointmentfieldtype Example](https://apis.io/examples/drchrono/customappointmentfieldtype-example/)
- [Custominsuranceplanname Example](https://apis.io/examples/drchrono/custominsuranceplanname-example/)
- [Custompatientfieldtype Example](https://apis.io/examples/drchrono/custompatientfieldtype-example/)
- [Customvitaltype Example](https://apis.io/examples/drchrono/customvitaltype-example/)
- [Doctor Example](https://apis.io/examples/drchrono/doctor-example/)
- [Doctorfeeschedule Example](https://apis.io/examples/drchrono/doctorfeeschedule-example/)
- [Doctormessage Example](https://apis.io/examples/drchrono/doctormessage-example/)
- [Doctoroptions Example](https://apis.io/examples/drchrono/doctoroptions-example/)
- [Eobobject Example](https://apis.io/examples/drchrono/eobobject-example/)
- [Feeschedule Example](https://apis.io/examples/drchrono/feeschedule-example/)
- [Implantabledevice Example](https://apis.io/examples/drchrono/implantabledevice-example/)
- [Insurance Example](https://apis.io/examples/drchrono/insurance-example/)
- …and 44 more published on APIs.io

#### Collections (6)

Postman and Open Collection request collections.

- [DrChrono EHR REST Administrative API](https://apis.io/collections/drchrono/open-drchrono-administrative-api/)
- [DrChrono EHR REST Administrative Audit API](https://apis.io/collections/drchrono/open-drchrono-audit-api/)
- [DrChrono EHR REST Administrative Availability API](https://apis.io/collections/drchrono/open-drchrono-availability-api/)
- [DrChrono EHR REST Administrative Billing API](https://apis.io/collections/drchrono/open-drchrono-billing-api/)
- [DrChrono EHR REST Administrative Clinical API](https://apis.io/collections/drchrono/open-drchrono-clinical-api/)
- [DrChrono EHR REST Administrative Practice Management API](https://apis.io/collections/drchrono/open-drchrono-practice-management-api/)

#### Overlays (12)

OpenAPI overlays, including conversational phrasing for each operation.

- [Drchrono Administrative Api Overlay](https://apis.io/overlays/drchrono/drchrono-administrative-api-overlay/)
- [Drchrono Administrative Api Phrasing Overlay](https://apis.io/overlays/drchrono/drchrono-administrative-api-phrasing-overlay/)
- [Drchrono Audit Api Overlay](https://apis.io/overlays/drchrono/drchrono-audit-api-overlay/)
- [Drchrono Audit Api Phrasing Overlay](https://apis.io/overlays/drchrono/drchrono-audit-api-phrasing-overlay/)
- [Drchrono Availability Api Overlay](https://apis.io/overlays/drchrono/drchrono-availability-api-overlay/)
- [Drchrono Availability Api Phrasing Overlay](https://apis.io/overlays/drchrono/drchrono-availability-api-phrasing-overlay/)
- [Drchrono Billing Api Overlay](https://apis.io/overlays/drchrono/drchrono-billing-api-overlay/)
- [Drchrono Billing Api Phrasing Overlay](https://apis.io/overlays/drchrono/drchrono-billing-api-phrasing-overlay/)
- [Drchrono Clinical Api Overlay](https://apis.io/overlays/drchrono/drchrono-clinical-api-overlay/)
- [Drchrono Clinical Api Phrasing Overlay](https://apis.io/overlays/drchrono/drchrono-clinical-api-phrasing-overlay/)
- [Drchrono Practice Management Api Overlay](https://apis.io/overlays/drchrono/drchrono-practice-management-api-overlay/)
- [Drchrono Practice Management Api Phrasing Overlay](https://apis.io/overlays/drchrono/drchrono-practice-management-api-phrasing-overlay/)

#### Governance rules (2)

Spectral rulesets for the provider's contracts.

- [drchrono — jsonschema rules](https://apis.io/rules/drchrono/drchrono-jsonschema-spectral-rules/)
- [drchrono API Rules](https://apis.io/rules/drchrono/drchrono-rules/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Drchrono Authentication](https://apis.io/security/drchrono/drchrono-authentication/)
- [Drchrono Domain Security](https://apis.io/security/drchrono/drchrono-domain-security/)
- [Drchrono Vulnerability Disclosure](https://apis.io/security/drchrono/drchrono-vulnerability-disclosure/)

#### OAuth scopes (2)

The permissions the API asks for.

- [Drchrono Fhir Smart Scopes](https://apis.io/scopes/drchrono/drchrono-fhir-smart-scopes/)
- [Drchrono Scopes](https://apis.io/scopes/drchrono/drchrono-scopes/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Drchrono Plans Pricing](https://apis.io/plans/drchrono/drchrono-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Drchrono Rate Limits](https://apis.io/rate-limits/drchrono/drchrono-rate-limits/)

#### FinOps (1)

What the API costs to run, by operation.

- [Drchrono Finops](https://apis.io/finops/drchrono/drchrono-finops/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.drchrono.com)
- [Documentation](https://app.drchrono.com/api-docs/tutorial/)
- [Getting started](https://app.drchrono.com/api-docs/tutorial/)
- [API reference](https://app.drchrono.com/api-docs/)
- [Pricing](https://www.drchrono.com/plans-and-pricing/)
- [Sign up](https://www.drchrono.com/sign-up/)
- [Support](https://support.drchrono.com/home/api)
- [Blog](https://www.drchrono.com/resources/blog/)
- [Terms of service](https://app.drchrono.com/api-terms/)
- [Privacy policy](https://www.drchrono.com/privacy-policy/)
- [Status](https://status.drchrono.com/)

## API Evangelist rating

**Kin Score 77.0** — exemplar · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 73.2 |
| Contract quality | 70.1 |
| Contract governance | 35.6 |
| Operational transparency | 84.2 |
| Developer ergonomics | 70.8 |
| Access clarity | 84.2 |

**Agent readiness 40.0** — agent-ready (gated down from agent-native)

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | served (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | partial |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | derived (partial credit) |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | served (partial credit) |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | partial |
| Operation Distinctiveness | distinct |

**Regulatory alignment 56.3** — Health

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/drchrono/](https://apis.io/providers/drchrono/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for drchrono: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/drchrono/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
