<p align="center">
  <strong>NeurAI</strong><br />
  <em>Clinician-facing dementia-screening workflow support</em>
</p>

<p align="center">
  <a href="#project-status">Project status</a> ·
  <a href="#what-neurai-does">What it does</a> ·
  <a href="#quick-start">Quick start</a> ·
  <a href="docs/">Documentation</a>
</p>

---

> **Demo only. Not for clinical use.** NeurAI is a prototype for a hackathon demonstration. It does not diagnose dementia, calculate clinical risk, or provide care advice. Use synthetic data only. Do not enter real patient information.

## Project status

NeurAI is planned as a public demonstration site. The repository is intended to be rebuilt from a clean start. The project brief describes the target architecture and safeguards. It does not prove that the source code implements them.

| Area | Current documented state |
|---|---|
| Web app | Next.js App Router and TypeScript; planned deployment on Vercel |
| Demo data | Synthetic fixtures; in-memory demo adapter is the stated default |
| Database and identity | Supabase is proposed. It is not provisioned. |
| AI | Gemini and Grok are proposed behind a server-side adapter. Live calls are disabled until approved. |
| Patient data | No real patient data. No raw media storage by default. |
| Clinical claims | No diagnosis, risk score, treatment advice, or clinical validation claim. |

**Do not deploy, provision cloud services, configure live AI, or use real patient data until the project owner approves the required legal, privacy, security, and operational reviews.** See [deployment](docs/DEPLOYMENT.md).

## What NeurAI does

NeurAI supports a proposed clinician workflow for dementia screening. The first release is planned to include:

- A dashboard.
- Patient list, create, and detail screens.
- An assessment workflow shell.
- Results display.
- An administrator status screen.
- A voice-capture interface prototype, if approved.

The product supports workflow demonstration only. It does not establish that any screening method is clinically valid. Do not treat a displayed result as a diagnosis or a recommendation.

## Who it is for

The documented roles are:

- **Clinician:** manages patient records and assessments within an assigned clinic.
- **Administrator:** manages clinics, users, roles, and audit metadata with least privilege.
- **Patient:** views their own history, sessions, and appointments. Patient access is read-only.

These are intended product rules. Verify them in the implementation before relying on them.

## Quick start

### Requirements

- Node.js 22.
- npm.

### Install and run

```bash
git clone git@github.com:rehaan-ahmad/NeurAI.git
cd NeurAI
npm install
cp .env.example .env.local
npm run dev
```

Open the local URL shown by Next.js. The project brief identifies `DEMO_MODE=true` as the local default. Confirm the behavior in the implementation before use. Use synthetic data only.

### Check the project

```bash
npm run lint
npm run typecheck
npm test
npm run build
```

Run all checks before you propose or perform a deployment. Do not assume that a successful build means the security controls have passed review.

## Configuration

Keep credentials out of source control. Add real credentials only to a secret store approved for the environment. Never expose a server secret to a browser component.

The project brief lists these variable names:

| Variable | Use | State |
|---|---|---|
| `DEMO_MODE` | Selects demo or production behavior | Use `true` for local demo. Production requires approval. |
| `NEXT_PUBLIC_APP_URL` | Public application URL | Set for the target environment. |
| `NEXT_PUBLIC_SUPABASE_URL` | Supabase endpoint | Only after Supabase is approved and provisioned. |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | Supabase browser key | Only after Supabase is approved and provisioned. |
| `SUPABASE_SERVICE_ROLE_KEY` | Server-side Supabase access | Secret. Server-side only. |
| `GEMINI_API_KEY` | Gemini server-side access | Secret. Leave unset until approved. |
| `GROK_API_KEY` | Grok server-side access | Secret. Leave unset until approved. |
| `NEXTAUTH_SECRET` | Authentication secret, if NextAuth is used | Secret. Server-side only. |

Use `.env.example` for names and safe placeholders only. The actual repository may differ. Check its configuration schema before setting values.

## Architecture at a glance

```text
Browser
  └─ Next.js routes and accessible UI
       └─ Server-only auth, services, repositories, audit, and AI adapter
            ├─ Demo adapter with synthetic fixtures (default)
            ├─ Supabase (proposed; not provisioned)
            └─ Gemini / Grok (proposed; disabled until approved)
```

The browser must not call an AI provider directly. Server-side code must check identity, role, clinic scope, and patient scope on each protected operation. See [architecture](docs/ARCHITECTURE.md) and [security](docs/SECURITY.md).

## Data and privacy

- Use synthetic fixtures during development and demonstration.
- Collect only data needed for the approved workflow.
- Do not upload or retain raw audio or video by default.
- Do not send identifiable health information to an external AI provider without explicit approval, informed consent, suitable contracts, and a reviewed data region and retention policy.
- Define retention, deletion, and export rules before any production data is stored.
- Do not claim legal or regulatory compliance based on code alone.

See [data lifecycle](docs/DATA-LIFECYCLE.md).

## Security reporting

Do not include patient information, secrets, or sensitive logs in a public issue. For a suspected vulnerability, contact the repository owner through a private channel. Rotate any exposed credential at once. See [security guidance](docs/SECURITY.md).

## Contributing

Before you propose a change:

1. Use synthetic data only.
2. Do not add secrets or real patient information.
3. Add or update tests for the change.
4. Run lint, type checking, tests, and a production build.
5. Document any new data collection, external service, or security boundary.
6. Do not provision or deploy a service without owner approval.

See [contribution guidance](CONTRIBUTING.md).

## Documentation map

- [Architecture](docs/ARCHITECTURE.md)
- [Data lifecycle](docs/DATA-LIFECYCLE.md)
- [Deployment](docs/DEPLOYMENT.md)
- [Product decisions and open items](docs/PRODUCT-DECISIONS.md)
- [Security](docs/SECURITY.md)
- [Contributing](CONTRIBUTING.md)

## License

No license is specified in the supplied project brief. Add a license only after the project owner selects one.
