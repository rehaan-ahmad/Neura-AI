# Contributing

Thank you for helping improve NeurAI. The project is a demonstration prototype. It is not a diagnostic system.

## Before you start

- Use synthetic data only.
- Do not add real patient information, recordings, transcripts, or credentials.
- Do not provision cloud resources or deploy without owner approval.
- Discuss changes to data collection, external providers, authentication, tenancy, or clinical claims before implementation.

## Change process

1. Create a focused branch for your change.
2. Keep the change small and clear.
3. Add or update tests.
4. Update the documentation when behavior or configuration changes.
5. Review the change for privacy, authorization, and tenant-isolation effects.
6. Run the checks below.
7. Submit a pull request with a summary, test results, and any known limitations.

## Local checks

```bash
npm run lint
npm run typecheck
npm test
npm run build
```

Confirm that each command exists in the current repository. Report any command that you cannot run. Do not say that a check passed unless you ran it.

## Documentation style

Use short sentences and familiar words. Use one term for one concept. State requirements directly. Separate confirmed behavior from plans and assumptions. Avoid unsupported clinical, security, or compliance claims.

## Security issues

Do not report a vulnerability in a public issue if the report contains exploit details, secrets, or sensitive data. Contact the repository owner through a private channel. Use synthetic data when you prepare a reproduction.
