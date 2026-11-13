# Setup and deployment

## Important

The project brief describes a local demo and a proposed deployment path. It does not confirm that the current source code or infrastructure implements these steps.

**Do not create cloud resources, configure live providers, use real patient data, or deploy without explicit owner approval.** The intended default is demo mode with synthetic fixtures.

## Requirements

The brief specifies:

- Node.js 22.
- npm.
- A Vercel account for the proposed frontend deployment.
- A Supabase project only if the backend is approved.
- AI provider credentials only after explicit approval.

## Local setup

```bash
git clone git@github.com:rehaan-ahmad/NeurAI.git
cd NeurAI
npm install
cp .env.example .env.local
npm run dev
```

Use synthetic data only. The project brief says `DEMO_MODE=true` is the local default. Check `.env.example` and the application configuration to confirm the actual default. Do not set production mode during local setup.

## Environment variables

The project brief lists the following names. Confirm the current code's schema before use.

| Variable | Purpose | Handling |
|---|---|---|
| `DEMO_MODE` | Select demo or production behavior | Keep `true` for local demo. |
| `NEXT_PUBLIC_APP_URL` | Application URL | Set per environment. |
| `NEXT_PUBLIC_SUPABASE_URL` | Supabase project URL | Use only after backend approval. |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | Supabase public client key | Use only after backend approval. |
| `SUPABASE_SERVICE_ROLE_KEY` | Server-side privileged access | Secret. Never expose to the browser. |
| `GEMINI_API_KEY` | Gemini provider access | Secret. Keep unset until approved. |
| `GROK_API_KEY` | Grok provider access | Secret. Keep unset until approved. |
| `NEXTAUTH_SECRET` | NextAuth secret, if applicable | Secret. Keep on the server. |

Copy `.env.example` to `.env.local` for local work. Do not commit `.env.local` or real credentials. A variable with a `NEXT_PUBLIC_` prefix can be exposed to the browser. Do not use that prefix for secrets.

## Local checks

The brief lists these scripts:

```bash
npm run dev
npm run build
npm run start
npm run lint
npm run typecheck
npm run test
```

Run the checks before a deployment:

```bash
npm install
npm run lint
npm run typecheck
npm test
npm run build
```

Confirm that each script exists in `package.json`. Resolve failures before deployment. A passing build does not replace a security or clinical review.

## Vercel deployment

This is a documented target, not an executed deployment.

1. Obtain explicit owner approval.
2. Confirm that the repository and branch are correct.
3. Import the repository into an approved Vercel project.
4. Select the Next.js framework preset.
5. Use the project build command. The brief specifies `npm run build`.
6. Add only approved environment variables through the hosting provider's secret settings.
7. Configure the production domain and TLS.
8. Configure authentication callback URLs for the production domain.
9. Review data region, access control, monitoring, backups, rollback, and incident contacts.
10. Deploy only after the checklist below is complete.

Do not configure a production identity callback to use `localhost`.

## Supabase backend

Supabase is proposed. It is not provisioned according to the project brief.

Before setup, obtain approval for data region, privacy, legal terms, access controls, retention, and operational ownership. Then:

1. Create the approved project in an approved region.
2. Review the schema and RLS SQL before applying it.
3. Configure authentication, role assignment, and recovery requirements.
4. Set the project URL and keys in approved secret settings.
5. Enable RLS and test least-privilege, clinic-scoped access.
6. Test patient scope on every read and write.
7. Configure private object storage only if media retention is approved.
8. Configure backups and document a tested restore and deletion process.

The brief references `src/server/persistence/rls.sql` and `tests/persistence.test.ts`. Confirm that these paths exist and that the SQL and tests are reviewed before use.

## AI providers

AI is disabled until the owner approves credentials, consent, data processing terms, region, retention, and cost controls. Provider calls must remain server-side. The app must show a visible unavailable state when AI is not configured.

Do not send identifiable health data to a provider during development or demonstration.

## Rollback and backups

Document and test the restore process before production data is stored. The brief proposes reverting the Git commit and redeploying a prior Vercel deployment for an application rollback. Review database migrations separately. Do not assume that an application rollback reverses a database change.

## Pre-deployment checklist

- [ ] Owner approval is recorded.
- [ ] Legal, privacy, security, and data-region reviews are complete.
- [ ] Demo mode and synthetic fixtures are clearly identified.
- [ ] No real patient data is present in fixtures, logs, URLs, or screenshots.
- [ ] Authentication and server-side authorization are tested.
- [ ] Clinic and patient scopes are tested.
- [ ] RLS is enabled and reviewed, if Supabase is used.
- [ ] Secrets are stored outside Git and are not exposed to the browser.
- [ ] AI remains disabled unless all approvals are complete.
- [ ] Media is not stored unless retention is approved.
- [ ] Backups, restore, deletion, monitoring, and rollback are documented and tested.
- [ ] `npm run lint`, `npm run typecheck`, `npm test`, and `npm run build` pass.
- [ ] No diagnosis, risk, efficacy, or compliance claim appears in the product without evidence and approval.
