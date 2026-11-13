# Architecture

## Purpose

This document describes the target architecture in the supplied project brief. It is not an audit of a source repository. Verify each item against the implementation before release.

## System overview

| Layer | Target technology or responsibility | State in project brief |
|---|---|---|
| Web application | Next.js App Router and TypeScript | Planned |
| Hosting | Vercel | Planned; no deployment performed |
| Persistence and identity | Supabase Postgres, Auth, row-level security (RLS), and private object storage | Proposed; not provisioned |
| AI | Server-side adapter for Gemini and Grok | Proposed; disabled until approved |
| Demo persistence | Typed adapter with synthetic fixtures | Stated local default; verify in code |

## Module boundaries

The project brief proposes these boundaries:

```text
src/
├── app/                         Routes and layouts
├── features/
│   ├── patients/                Patient workflow UI and logic
│   ├── assessments/             Assessment workflow UI and logic
│   ├── results/                 Results UI and logic
│   └── admin/                   Administration UI and logic
├── server/
│   ├── auth/                    Server-side identity and authorization
│   ├── services/                Business services
│   ├── repositories/            Data access
│   ├── ai/                      Server-only AI integration
│   └── audit/                   Audit event handling
├── lib/
│   ├── schemas/                 Shared validation schemas
│   ├── types/                   Shared types
│   └── config/                  Configuration
└── tests/                       Unit, integration, and end-to-end tests
```

Use server components by default. Use client components only when browser APIs require them, such as media capture or voice interaction. Do not import server secrets into client components.

## Data access and tenancy

The target supports multiple clinics. Treat each clinic as a tenant.

For every protected read or write:

1. Authenticate the user on the server.
2. Check the user's role and organization membership.
3. Check the requested patient or record belongs to an organization the user may access.
4. Apply the same restrictions in the persistence layer with RLS when Supabase is used.
5. Record approved mutations in the audit system without copying sensitive payloads.

A hidden button is not an authorization control. A client-supplied clinic or patient identifier is not proof of access.

Patient users have read-only access to their own history, sessions, and appointments. They must not create records or change clinical data.

## Persistence adapter

Keep persistence behind a typed interface. Use the synthetic demo adapter until a production backend is approved and provisioned. The interface and user experience must make demo mode clear. Do not allow demo data to appear as production data.

The project brief says demo data is in memory and does not persist across server restarts. Confirm that behavior in the code. Do not assume that an in-memory adapter is suitable for production.

## Authentication and authorization

Check authorization on the server for every protected route, server action, API route, and data operation. Do not use a hard-coded mock identity in production. A local authentication emulator is acceptable only when a developer explicitly configures it and the app visibly identifies demo mode.

Administrator routes require the administrator role. Apply least privilege to all roles.

## AI boundary

All provider calls must pass through a server-side adapter. The browser must not call Gemini, Grok, or another provider directly.

The adapter must:

- Validate input before a provider call.
- Limit payload size and request rate.
- Apply a timeout.
- Validate the provider response against a schema.
- Return safe errors that do not reveal secrets or sensitive data.
- Avoid sensitive data in logs.
- Record provider, model, version, uncertainty, and human-review state where approved.
- Show a clear unavailable state when credentials are missing.

AI must not diagnose, calculate an unvalidated composite score, or provide care advice. Enable external AI only after the owner approves consent, contracts, processing terms, region, retention, and budget controls.

## Media boundary

Request microphone or camera permission only after a user action. Handle permission denial, interruption, cancellation, and upload failure. Stop media tracks when capture ends.

Do not retain or upload raw recordings by default. If the owner approves media retention, use private object storage and short-lived signed access. Do not use public media URLs.

## Audit boundary

Use append-only audit events for approved mutations. Restrict access to audit data. Store metadata needed for review; do not copy raw clinical payloads into audit events. A deletion operation may remove record references while retaining approved audit metadata.

## Configuration and secrets

Validate configuration at startup. Keep server secrets on the server. Commit variable names and safe placeholders only. Do not commit real credentials. See [deployment](DEPLOYMENT.md) for the variables identified in the brief.

## Target screens

The product brief names these first-release screens:

- Dashboard.
- Patient list, create, and detail.
- Assessment shell.
- Results.
- Administrator status.

These are scope decisions, not proof that the screens exist in the current repository.
