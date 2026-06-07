# Cloudflare API Token Setup

Use this reference when the user needs to create credentials for Cloudflare Pages deployment.

## What the user needs

For the workflow in this skill, the user typically needs:

- a Cloudflare account
- an API token with Cloudflare Pages permissions
- the Cloudflare Account ID for the target account

Official token page:

- https://dash.cloudflare.com/profile/api-tokens

## Safe guidance rules

- Explain the application path and required permissions.
- Never generate or display a fake token value that looks real.
- Never ask the user to paste a secret into a public repository.
- Recommend environment variables or a private local config file.

## Token application path

1. Open https://dash.cloudflare.com/profile/api-tokens.
2. Sign in to Cloudflare if prompted.
3. Select `Create Token`.
4. Choose `Create Custom Token` when the user wants the narrowest practical scope.
5. Name the token, for example `pages-deploy`.
6. Add the permissions listed below.
7. Limit the account scope to the intended Cloudflare account.
8. Create the token and copy it once.

## Recommended token scope

Use the narrowest token that supports Pages deployment for the intended account.

For this skill's normal deployment workflow, use:

| Scope | Permission group | Access |
| --- | --- | --- |
| Account | `Cloudflare Pages` | `Edit` |

This is the key permission Cloudflare documents for custom tokens that use the Pages API.

For read-only inspection, deployment listing, or diagnostics, `Pages Read` is enough. For deployment and project creation, prefer `Cloudflare Pages: Edit`.

Resource scope:

- Include only the target account.
- Do not grant access to all accounts unless the user intentionally wants cross-account deployment.
- Do not add zone-level DNS permissions unless the workflow also needs custom domain or DNS changes.

Optional permissions:

- Custom domain binding may require additional domain or DNS permissions depending on the exact flow.
- This skill's default `pages.dev` deployment does not require DNS edit permissions.

Official references:

- Cloudflare Pages API: https://developers.cloudflare.com/pages/configuration/api/
- Cloudflare API token permissions: https://developers.cloudflare.com/fundamentals/api/reference/permissions/

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

For one-off terminal sessions, export them in the shell before running `wrangler`.

For persistent local use, store them in a private local config or shell profile that is not committed to Git.

For agent workflows, pass them through the environment. Do not paste the real token into a prompt unless the runtime is explicitly trusted for secrets.

## What to tell the user

Keep the explanation short and practical:

- where to click
- what to create
- which permission to select
- how to store it safely
- how to avoid leaking it into GitHub
