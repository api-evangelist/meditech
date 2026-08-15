---
name: meditech-patient-record-search
description: >-
  Search and read a MEDITECH Greenfield patient's discrete clinical records (allergies,
  conditions, diagnostic reports, encounters, medications, observations) one resource type
  at a time via US Core FHIR R4.
api: meditech:meditech-patient-api
operations:
  - searchPatients
  - getPatient
  - getPatientAllergies
  - getPatientConditions
  - getPatientDiagnosticReports
  - getPatientEncounters
  - getPatientMedications
  - getPatientObservations
generated: '2026-08-14'
method: generated
source:
  - openapi/meditech-patient-api-openapi.yml
  - openapi/meditech-allergy-api-openapi.yml
  - openapi/meditech-condition-api-openapi.yml
  - openapi/meditech-diagnostic-api-openapi.yml
  - openapi/meditech-encounter-api-openapi.yml
  - openapi/meditech-medication-api-openapi.yml
  - openapi/meditech-observation-api-openapi.yml
---

# MEDITECH Patient Record Search

Use this instead of `$everything` (see the companion
`meditech-patient-clinical-summary` skill) when you only need ONE resource type -- narrower
scope, smaller payload, and it only requires the scope for that resource
(e.g. `patient/Observation.read`) rather than the broad `patient/*.read` wildcard.

## Steps

1. **Resolve the patient id** with `searchPatients` (`GET /Patient?...`) if you don't already
   have it, then confirm with `getPatient` (`GET /Patient/{id}`).
2. **Pick the resource-specific call** based on what the caller actually needs, each nested
   under the resolved patient id per `data-model/meditech-data-model.yml` (every one of these
   `belongs_to` Patient via the `{id}` path segment):
   - `getPatientAllergies` — `GET /Patient/{id}/AllergyIntolerance`
   - `getPatientConditions` — `GET /Patient/{id}/Condition`
   - `getPatientDiagnosticReports` — `GET /Patient/{id}/DiagnosticReport`
   - `getPatientEncounters` — `GET /Patient/{id}/Encounter`
   - `getPatientMedications` — `GET /Patient/{id}/MedicationRequest`
   - `getPatientObservations` — `GET /Patient/{id}/Observation`
3. **Every response is a FHIR `Bundle`.** Paginate by following the Bundle's `link` (rel:
   `next`) entries verbatim rather than constructing your own page parameters
   (`conventions/meditech-conventions.yml`).
4. **Scope minimally.** Request only the `patient/{Resource}.read` scope for the resource(s)
   you actually call, not the `patient/*.read` wildcard, when your OAuth client registration
   allows choosing scopes -- narrower scopes are the safer default for an agent acting on a
   patient's behalf.
5. **All 6 resource types here are read-only** in this program -- there is no create/update
   path for AllergyIntolerance, Condition, DiagnosticReport, Encounter, MedicationRequest, or
   Observation (see `conformance/meditech-greenfield-conformance.yml` read_only_resources).
