# Cloudflare API Token Setup

Use this reference when the user needs to create credentials for Cloudflare Pages deployment.

## What the user needs

For the workflow in this skill, the user typically needs:

- a Cloudflare account
- an API token with Pages-related permissions
- the Cloudflare Account ID for the target account

## Safe guidance rules

- Explain the application path and required permissions.
- Never generate or display a fake token value that looks real.
- Never ask the user to paste a secret into a public repository.
- Recommend environment variables or a private local config file.

## Token application path

1. Sign in to the Cloudflare dashboard.
2. Open the user profile menu.
3. Go to `My Profile`.
4. Open the `API Tokens` section.
5. Choose `Create Token`.

## Recommended token scope

Use the narrowest token that supports Pages deployment for the intended account.

At minimum, guide the user to review:

- account scope for the correct Cloudflare account
- Pages permissions needed for project creation and deployment

If the user is unsure which exact permission preset to choose, tell them to start from the Cloudflare Pages deployment documentation and avoid granting broader account permissions than necessary.

## Account ID lookup

The Account ID is usually visible in the Cloudflare dashboard for the selected account.

Tell the user to store it privately and expose it to tooling through:

- `CLOUDFLARE_ACCOUNT_ID`
- a protected local config file not committed to Git

## Safe local configuration example

```bash
export CLOUDFLARE_API_TOKEN="your-real-token-here"
export CLOUDFLARE_ACCOUNT_ID="your-account-id-here"
```

These values are placeholders only. They should be set locally and never committed.

## What to tell the user

Keep the explanation short and practical:

- where to click
- what to create
- how to store it safely
- how to avoid leaking it into GitHub
