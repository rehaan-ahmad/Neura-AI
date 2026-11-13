# Data lifecycle

## Purpose and limit

NeurAI is a prototype for clinician workflow support. It is not a diagnostic system. This document records the intended data rules from the project brief. It does not establish legal or regulatory compliance.

**Do not enter real patient data.** Use synthetic data until the owner completes legal, privacy, security, and operational review and gives explicit approval.

## Data minimization

Collect only the minimum data that an approved workflow needs. The brief identifies these possible fields:

- Patient reference.
- Age and gender.
- Assessment session.
- Cognitive-test observations.
- Consent metadata.
- Clinician notes.

The final field list requires owner and privacy review. Do not add a field only because it is easy to collect.

Use synthetic development fixtures. Do not use realistic identifiers or real patient details in fixtures, test output, screenshots, logs, URLs, or issue reports.

## Media and transcripts

Raw audio, video, and transcripts are not stored by default. The proposed speech and gait capture are local demonstrations only. Media must remain in memory and must not be uploaded unless the owner approves the purpose, consent process, provider terms, location, retention, and access controls.

If retention is approved:

- Use private object storage.
- Do not create public URLs.
- Use short-lived signed access.
- Define deletion and backup behavior before collection.

## Retention

The project brief does not define a retention schedule. The responsible organization must approve a schedule before production data is stored. Do not infer a retention period from the application code.

The brief describes demo fixtures as ephemeral and in-memory. They should not persist across server restarts. Verify this behavior in the implementation.

## Access

Scope data to the authenticated user's clinic. Scope patient access to the patient's own records. Enforce these checks on the server and, if Supabase is provisioned, in RLS policies.

## Deletion

The brief describes server-side soft deletion for patient records in the demo adapter and append-only audit metadata. Verify both behaviors in code.

Production deletion must follow an approved organizational policy. The policy must define how deletion interacts with backups, audit records, and legal holds. Do not retain raw sensitive payloads in audit events.

## Export

The supplied brief states that no export function is implemented. Do not promise an export feature until it exists and has passed access-control and privacy review.

## External processing

Do not send identifiable health data to an external AI provider unless all required approvals are complete. Review consent, provider terms, data location, retention, security controls, and organizational authorization first.

## Compliance statement

Code alone does not establish compliance with any data-protection law or medical-device requirement. Obtain qualified jurisdiction-specific advice before any use with real patient data.
