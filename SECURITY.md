# Security policy

## Supported status

NeurAI is a prototype for demonstration. The project brief does not identify a supported production release. Do not use the application with real patient data.

## Report a vulnerability

Do not publish secrets, patient information, or exploit details in a public issue. Contact the repository owner through a private channel. Include:

- A brief description of the issue.
- The affected route, component, or dependency.
- The potential impact.
- Steps to reproduce with synthetic data.
- Any suggested mitigation.

Do not test against a live system unless the owner has given written authorization.

## If a secret is exposed

1. Revoke the exposed credential.
2. Create a replacement credential.
3. Update the approved secret store.
4. Review access logs for unexpected use.
5. Remove the secret from repository history when needed.
6. Notify the repository owner and affected service owners.

Do not assume that deleting a secret from the latest commit makes it safe. Treat exposed credentials as compromised.
