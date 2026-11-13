# Security

## Scope

NeurAI is a prototype for clinician workflow support. It is not a diagnostic system. This document describes target security controls from the project brief. It is not a security audit or a compliance certification.

**Do not use real patient data.** A production deployment requires qualified security, privacy, legal, and clinical review.

## Security requirements

### Authentication and authorization

- Authenticate each user on the server.
- Authorize every protected route, server action, API route, and data operation on the server.
- Check the user's role, clinic membership, and access to the requested patient record.
- Give patient users read-only access to their own records.
- Require the administrator role for administrator routes.
- Do not treat hidden UI controls as authorization.
- Do not use a hard-coded mock user in production.

### Tenant isolation

Treat each clinic as a separate tenant. Enforce clinic and patient scope on every read and write. If Supabase is used, enforce the policy in both server logic and RLS. Test attempts to access another clinic's records.

### Secrets

- Keep service-role keys, AI keys, and authentication secrets on the server.
- Do not commit `.env.local`, credentials, tokens, or private keys.
- Use safe placeholders in `.env.example`.
- Do not print secrets in logs, errors, URLs, screenshots, or support reports.
- If a secret is exposed, revoke and replace it. Review access logs and remove the exposed value from repository history as needed.

### Input and output

- Validate request data with explicit schemas.
- Use parameterized database queries or a reviewed ORM.
- Allow only approved fields in create and update operations.
- Return only fields required by the current user interface.
- Use framework-safe text rendering. Avoid unsafe HTML rendering. If rich HTML is required, use a reviewed sanitizer and a restrictive content security policy.
- Return safe errors. Do not expose stack traces, SQL details, credentials, or sensitive record data.

### AI boundary

- Send provider requests from server-side code only.
- Validate provider inputs and outputs.
- Set request timeouts and rate limits.
- Bound payload size.
- Do not place sensitive data in prompts or logs.
- Show an explicit unavailable state if credentials are missing.
- Record provider, model, version, uncertainty, and human-review state when approved.
- Do not present AI output as a diagnosis, validated risk score, or care recommendation.

### Media

Request capture permission only after a user action. Handle denial, interruption, cancellation, and failure. Stop all media tracks when capture ends. Do not retain or upload raw media by default. Use private storage and short-lived access only after retention approval.

### Logging and audit

Do not log patient names, identifiers, recordings, transcripts, or clinical payloads. Use append-only audit events for approved mutations. Restrict audit access and avoid copying sensitive payloads into events.

### Dependencies and build

Review new dependencies before installation. Check the package name, source, ownership, maintenance, and purpose. Pin and update dependencies through the project's normal review process. Run automated dependency and code scans where the repository supports them. A clean scan does not prove that the application is secure.

The supplied brief reports zero known production dependency vulnerabilities at the time it was written. This is a dated statement, not a current scan result. Run a fresh dependency audit before release.

## Security test checklist

- [ ] No secret is exposed to client-side code.
- [ ] Every protected operation checks authorization on the server.
- [ ] Clinic and patient scopes are checked on every read and write.
- [ ] Users cannot change fields outside their role.
- [ ] Responses expose only required fields.
- [ ] User input is validated and safely rendered.
- [ ] Errors and logs do not expose sensitive data.
- [ ] RLS policies are enabled and tested if Supabase is used.
- [ ] AI is server-side, bounded, validated, and disabled until approved.
- [ ] Raw media is not retained by default.
- [ ] Demo mode is visible and uses synthetic data only.
- [ ] Keyboard operation and accessible controls are tested.
- [ ] Lint, type check, tests, and production build pass.
- [ ] Security and privacy review is complete before production use.

## Reporting a vulnerability

Do not publish secrets, patient information, or exploit details in a public issue. Contact the repository owner through a private channel. Include the affected component, impact, and steps to reproduce with synthetic data only.

## Limitations

Code alone does not establish compliance with HIPAA, GDPR, Indian law, or other regulation. Do not claim certification or compliance without an independent assessment and documented evidence. Production use requires expert review, approved methods, governance, contracts, and jurisdiction-specific controls.
