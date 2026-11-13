# Product decisions and open items

## Status

This document summarizes the decisions in the supplied project brief. It separates confirmed decisions from assumptions and unresolved items. Confirm the list with the project owner before a production release.

## Confirmed decisions

| Topic | Decision |
|---|---|
| Intended use | Prototype for a hackathon demonstration. |
| Product claim | Dementia-screening workflow support only. No diagnosis, individual risk probability, treatment advice, or clinical efficacy claim. The interface must say that the product is not a diagnosis and is not clinically validated. |
| User roles | Clinician, administrator, and patient. |
| Patient access | Read-only access to the patient's own history, sessions, and appointments. |
| Clinic model | Multiple clinics. Each clinic is a tenant. |
| Jurisdiction | India, with rural and urban users in scope. |
| Data | Minimum necessary fields; synthetic development fixtures only. |
| Risk and advice | No invented composite risk score or AI-generated care advice. |
| Frontend hosting target | Vercel, for a public demonstration site. |
| Backend choice | Supabase is the proposed backend. It is not provisioned. Firebase is not in scope. |
| AI providers | Gemini and Grok are proposed behind a server-side adapter. Automatic model selection is a target. Live calls are disabled until approved. |
| Design target | Muted purple and teal palette, Inter font, responsive clinician application shell. |
| Initial screens | Dashboard; patient list, create, and detail; assessment shell; results; administrator status. |
| Starting point | Rebuild from a clean repository. No patient data migration is planned. |
| Release target | Public demo with no public patient health information. |

## Assumptions to confirm

- Compliance owner: assumed to be the product owner; no owner is assigned.
- Identity requirements: provider authentication is required before production. SSO, MFA, and invitations are not confirmed.
- Retention, deletion, and export rules: require legal and organizational review.
- Audit policy: least privilege and append-only audit metadata are proposed.
- Accessibility: WCAG 2.2 AA is a target, not a verified result.
- Interface language: English is an assumption.
- Supabase region: an India-compatible region is preferred, but the region is not selected.
- Vercel team, project, and domain: not supplied.
- Email sender, monitoring, and backup ownership: not supplied.
- AI credentials and budget limits: not supplied.

## Blockers before production

Do not enable production use until the owner resolves these items:

1. Assign an accountable compliance and privacy owner.
2. Complete legal, privacy, security, and clinical governance review for the intended jurisdiction.
3. Approve the data fields, consent process, retention schedule, deletion process, and export policy.
4. Select and approve the data region and hosting configuration.
5. Provision Supabase and verify authentication, RLS, and tenant isolation.
6. Approve any AI provider, processing terms, data sent, region, retention, consent, and spending limit.
7. Validate any cognitive test content and methods. Do not copy protected test content without permission.
8. Define the gait capture protocol. Do not infer clinical meaning from video without a validated method and approval.
9. Verify accessibility, backups, restore, monitoring, incident response, and rollback.
10. Obtain explicit deployment approval.

## Demo boundary

Demo mode must be visibly labeled. It must use synthetic fixtures. It must not be mistaken for a production system. Do not use real patient data, recordings, or transcripts in the demo.

## Non-goals

The current brief excludes diagnosis, clinical validation, compliance certification, raw media storage by default, AI-generated care advice, and patient creation by patient users.
