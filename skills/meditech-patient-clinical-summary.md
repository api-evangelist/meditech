---
name: meditech-patient-clinical-summary
description: >-
  Retrieve a patient's full clinical summary from MEDITECH's Greenfield US Core FHIR R4 API
  using the FHIR $everything operation, after completing SMART on FHIR authorization.
api: meditech:meditech-patient-api
operations:
  - getPatient
  - getPatientEverything
generated: '2026-08-14'
method: generated
source: openapi/meditech-patient-api-openapi.yml
---

# MEDITECH Patient Clinical Summary

Grounded in real `operationId`s from `openapi/meditech-patient-api-openapi.yml` -- this skill
does not invent an endpoint.

## Preconditions

1. You hold a valid SMART on FHIR bearer token from the `authorization_code` + PKCE (S256)
   flow against `https://greenfield-prod-apis.meditech.com/oauth/authorize` /
   `.../oauth/token` (see `authentication/meditech-greenfield-oauth.yml`).
2. Your token carries at least `patient/*.read` scope (or the specific
   `patient/{Resource}.read` scopes for what you intend to pull -- see
   `scopes/meditech-scopes.yml`).
3. This program is READ-ONLY for Patient. There is no `create` interaction for Patient --
   do not attempt to register a patient through this API.

## Steps

1. **Confirm the patient FHIR id.** If you only have a demographic (name/DOB), call
   `searchPatients` (`GET /Patient?name=...&birthdate=...`) first to resolve the id. Do not
   guess or construct an id.
2. **Call `getPatient`** — `GET /Patient/{id}` — to confirm the resource exists and you can
   read it before pulling the full summary. A `404` here means the id is wrong or not visible
   in your authorization context; do not retry with a different guessed id.
3. **Call `getPatientEverything`** — `GET /Patient/{id}/$everything` — to retrieve the bundled
   clinical summary (returns a `Bundle` aggregating the patient's available resources).
4. **Handle errors per `errors/meditech-problem-types.yml`**: a `401` means your token is
   missing/expired -- re-run the OAuth flow, don't retry the same token. There is no documented
   numeric rate limit or `Retry-After` header (see `rate-limits/meditech-rate-limits.yml`) --
   back off conservatively on repeated failures rather than assuming a specific window.
5. **Treat PHI accordingly.** Every field returned is protected health data under HIPAA (see
   `agentic-access/meditech-agentic-access.yml` -- `consequence: read`, but still PHI). Do not
   log, cache, or forward the response body outside the authorized application boundary.

## What this skill does NOT cover

- Writing data back (only `Communication` create and `QuestionnaireResponse` create/update are
  writable in this program -- see `conformance/meditech-greenfield-conformance.yml`).
- Scheduling (`FHIR Scheduling` is listed "coming soon" in Greenfield; see
  `lifecycle/meditech-lifecycle.yml`).
