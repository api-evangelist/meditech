# MEDITECH (meditech)

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

MEDITECH (Medical Information Technology, Inc.) is an electronic health record vendor serving community hospitals and health systems, primarily through its MEDITECH Expanse platform. Its API program is delivered through the Greenfield Workspace — a registration-gated developer environment where approved app developers get interactive documentation and a sandbox to execute APIs against a real MEDITECH EHR. Published surfaces are US Core FHIR R4 (view-only patient-facing data, USCDI v1, DSTU2/R4 compatible) and FHIR Scheduling APIs. MEDITECH also operates Traverse Exchange, its national data exchange network and TEFCA on-ramp, connecting 700+ facilities across 41 US states plus Canadian deployments.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/meditech/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/meditech/refs/heads/main/apis.yml)

> ## ⚠️ Read this before building on this repo
>
> **The OpenAPI definitions here are API Evangelist models of US Core FHIR R4 — not MEDITECH-published contracts.**
>
> MEDITECH does not publish an OpenAPI. Everything in `openapi/` was modeled from the FHIR R4
> standard MEDITECH states it supports, and the `authentication/`, `scopes/`, `agentic-access/`,
> and `collections/` artifacts are derived from those models — so they inherit the same provenance.
>
> **The authoritative reference is the Greenfield API Explorer** →
> **[greenfield.meditech.com/explorer/topic/welcome](https://greenfield.meditech.com/explorer/topic/welcome)**
>
> Reconcile everything in this repo against it. The sections that matter most:
>
> | Section | URL |
> |---|---|
> | Welcome / getting started | [`/explorer/topic/welcome`](https://greenfield.meditech.com/explorer/topic/welcome) |
> | Patient health data | [`/explorer/topic/patient-health-data`](https://greenfield.meditech.com/explorer/topic/patient-health-data) |
> | API reference | [`/explorer/api`](https://greenfield.meditech.com/explorer/api) |
> | **Endpoint directory** | [`/explorer/endpoints`](https://greenfield.meditech.com/explorer/endpoints) |
> | **Scope list** | [`/explorer/scope`](https://greenfield.meditech.com/explorer/scope) |
> | Authorization | [`/explorer/authorization`](https://greenfield.meditech.com/explorer/authorization) |
> | Status codes | [`/explorer/status-codes`](https://greenfield.meditech.com/explorer/status-codes) |
>
> *(Route list confirmed from the Explorer's shipped Angular bundle, not guessed.)*
>
> Practical consequences:
>
> - **Service base URLs are per-customer.** The `https://{facility}.meditech.com/fhir/r4` template
>   reflects MEDITECH's deployment model. Real endpoints are issued per organization.
> - **OAuth endpoints are placeholders.** Read each deployment's `.well-known/smart-configuration`.
> - **Operation coverage is unverified.** FHIR compartment searches (`/Patient/{id}/Observation`)
>   and `$everything` are optional server capabilities. Check the facility's
>   `CapabilityStatement` at `/metadata` before binding anything to them.
> - **PHI applies.** Every operation modeled here reads protected health data under HIPAA.
>
> **Why we couldn't just read the Explorer and fix this automatically:** it is an Angular
> single-page app that renders entirely client-side, and `greenfield.meditech.com` serves
> `robots.txt` with `Disallow: /`. The docs are open to a human with a browser but invisible to
> crawlers, indexes, and any AI agent that fetches URLs or respects robots. That's a
> *machine-discoverability* problem, not an access problem — and it's precisely why a derived
> scaffold ended up standing in for the real contract here.
>
> Register for sandbox access (executing calls against a real EHR does require approval) at
> [ehr.meditech.com/ehr-solutions/greenfield-workspace](https://ehr.meditech.com/ehr-solutions/greenfield-workspace).
>
> *Corrections and evidence-backed PRs welcome — especially transcriptions of
> `/explorer/endpoints` and `/explorer/scope`, which should supersede the derived artifacts here.*

## ⛔ There is no write access

**As of 2026-07-27, every API MEDITECH exposes through Greenfield Workspace is read-only.**
There is no supported public path to create a patient, book an appointment, or reschedule one.
If you applied for API access and were granted read-only, that is the expected outcome — not a
sign you applied to the wrong program.

| Surface | Access | Workflow | Available to |
|---|---|---|---|
| [FHIR Patient Access — DSTU2](https://greenfield.meditech.com/explorer/topic/patient-health-data) | **Read-only** | Patient-facing | All customers |
| [FHIR Patient Access — R4 / US Core](https://greenfield.meditech.com/explorer/topic/USCore-patient-health-data) | **Read-only** | Patient-facing | All customers |
| [HL7v2 Interfaces](https://ehr.meditech.com/hl7-outbound-list-for-greenfield) | **Read-only** | **Outbound only** (ADT, labs, meds, orders) | All customers |
| FHIR Scheduling APIs | *Coming soon* | — | **Expanse customers only** |

Scheduling write-back **does** exist in Expanse — MEDITECH markets Argo-Scheduling (FHIR STU3:
retrieve slots, book visits, manage provider availability), and partners like Luma Health have
shipped against it. But that is delivered through **customer-sponsored or formal-partner
integration at the hospital**, not through self-serve Greenfield access. Greenfield's own
resources page still lists FHIR Scheduling as "coming soon."

Practical read: if your use case is writes, the route is a MEDITECH customer sponsoring the
integration or a partner agreement — not the developer program.

## Access

| | |
|---|---|
| **Pricing** | Unknown — none published |
| **Onboarding** | Registration request (reviewed by MEDITECH), not self-serve |
| **Public docs** | Yes, but machine-invisible — [Greenfield API Explorer](https://greenfield.meditech.com/explorer/topic/welcome) is client-rendered and robots-disallowed |
| **Sandbox** | Approval required (Greenfield Workspace) |
| **Try now** | No |

## Timestamps

- **Created:** 2026-05-04
- **Modified:** 2026-07-27
- **Provenance reviewed:** 2026-07-27

## APIs

### MEDITECH Expanse FHIR API

MEDITECH's FHIR API surface for Expanse, exposed to approved developers through the Greenfield Workspace. US Core FHIR R4 provides view-only access to patient-facing data after the patient authorizes the requesting app (USCDI v1, DSTU2/R4 compatible, patient workflows only). Separate FHIR Scheduling APIs support user and patient workflows for Expanse customers. Service base URLs are issued per customer facility.

- **Human URL:** [https://ehr.meditech.com/ehr-solutions/greenfield-workspace](https://ehr.meditech.com/ehr-solutions/greenfield-workspace)
- **Base URL:** `https://{facility}.meditech.com/fhir/r4` *(template — issued per organization)*

#### Tags

- EHR
- Healthcare
- FHIR
- HL7

#### Properties

- [Documentation](https://greenfield.meditech.com/explorer/topic/welcome)
- [Endpoints](https://greenfield.meditech.com/explorer/endpoints)
- [Scopes](https://greenfield.meditech.com/explorer/scope)
- [Authorization](https://greenfield.meditech.com/explorer/authorization)
- [Status Codes](https://greenfield.meditech.com/explorer/status-codes)
- [Sign Up](https://ehr.meditech.com/ehr-solutions/greenfield-workspace)

### Resource APIs *(AE-derived, one per FHIR resource)*

Split by tag from the source model in `openapi/_original/meditech-fhir-openapi.yml`. **All read
operations** — consistent with MEDITECH's actual read-only surface. No scheduling API is modeled
here because MEDITECH does not yet expose one through Greenfield.

| API | Covers | OpenAPI |
|---|---|---|
| Allergy | Allergy and intolerance records | [`meditech-allergy-api-openapi.yml`](openapi/meditech-allergy-api-openapi.yml) |
| Capability | FHIR server capability statement | [`meditech-capability-api-openapi.yml`](openapi/meditech-capability-api-openapi.yml) |
| Condition | Problem list and diagnoses | [`meditech-condition-api-openapi.yml`](openapi/meditech-condition-api-openapi.yml) |
| Diagnostic | Diagnostic reports (lab, radiology, pathology) | [`meditech-diagnostic-api-openapi.yml`](openapi/meditech-diagnostic-api-openapi.yml) |
| Encounter | Clinical encounters and visits | [`meditech-encounter-api-openapi.yml`](openapi/meditech-encounter-api-openapi.yml) |
| Medication | Medication requests and prescriptions | [`meditech-medication-api-openapi.yml`](openapi/meditech-medication-api-openapi.yml) |
| Observation | Vital signs and laboratory results | [`meditech-observation-api-openapi.yml`](openapi/meditech-observation-api-openapi.yml) |
| Patient | US Core Patient resources | [`meditech-patient-api-openapi.yml`](openapi/meditech-patient-api-openapi.yml) |

## Common Properties

- [LinkedIn](https://www.linkedin.com/company/meditech)
- [Portal](https://greenfield.meditech.com/) — Greenfield Workspace
- [Website](https://www.meditech.com/)
- [Documentation](https://greenfield.meditech.com/explorer/topic/welcome) — Greenfield API Explorer
- [API Reference](https://greenfield.meditech.com/explorer/api)
- [Endpoints](https://greenfield.meditech.com/explorer/endpoints)
- [Scopes](https://greenfield.meditech.com/explorer/scope)
- [Authorization](https://greenfield.meditech.com/explorer/authorization)
- [Status Codes](https://greenfield.meditech.com/explorer/status-codes)
- [Sign Up](https://ehr.meditech.com/ehr-solutions/greenfield-workspace)
- [Getting Started](https://ehr.meditech.com/ehr-solutions/how-to-work-in-the-greenfield-workspace)
- [Resources](https://ehr.meditech.com/ehr-solutions/greenfield-workspace-resources)
- [Interoperability](https://ehr.meditech.com/ehr-solutions/meditech-interoperability) — Traverse Exchange / TEFCA
- [Blog](https://blog.meditech.com/)
- [Support](https://ehr.meditech.com/contact)
- [Privacy Policy](https://ehr.meditech.com/privacy-policy)
- [OpenAPI](https://raw.githubusercontent.com/api-evangelist/meditech/refs/heads/main/openapi/_original/meditech-fhir-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [JSON Schema](https://raw.githubusercontent.com/api-evangelist/meditech/refs/heads/main/json-schema/meditech-patient-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON-LD Context](https://raw.githubusercontent.com/api-evangelist/meditech/refs/heads/main/json-ld/meditech-context.jsonld)
- [Postman Collection](collections/meditech-fhir.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/meditech-fhir.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

## Not published by MEDITECH

Honestly absent — recorded here so their absence is not mistaken for an oversight:

- No public API pricing or plan tiers
- No published rate limits or rate-limit response headers
- No machine-readable OpenAPI, capability statement, or downloadable spec
- No `security.txt`, bug bounty, or trust center
- No public API changelog or status page

The endpoint directory and scope list **do** exist — at [`/explorer/endpoints`](https://greenfield.meditech.com/explorer/endpoints)
and [`/explorer/scope`](https://greenfield.meditech.com/explorer/scope) — but only as rendered
HTML behind a robots-disallowed SPA, so nothing can consume them programmatically.

Earlier scaffold artifacts that *asserted* pricing, plan tiers, and rate limits have been
quarantined in [`_scaffold/`](_scaffold/) — see the note there.
