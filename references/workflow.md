# Workflow Reference

## Purpose

Use this reference when executing or explaining the OpenDesign-style deployment workflow for static HTML on Cloudflare Pages.

## Deployment Sequence

### 1. Confirm inputs

- Required:
  - A local static site directory
  - `index.html` at the root or a clearly identified entrypoint
  - Cloudflare access through environment variables or a protected local config
- Optional:
  - Existing project name if the public URL must stay unchanged
  - Custom domain mapping if the user wants more than `pages.dev`

### 2. Preferred environment setup

- Install `wrangler` if it is missing.
- Provide credentials through environment variables instead of inline command literals.
- Keep token-bearing config files private and outside the repo when possible.

Suggested environment variables:

- `CLOUDFLARE_API_TOKEN`
- `CLOUDFLARE_ACCOUNT_ID`

If the user does not have a token yet, send them to [cloudflare-token-setup.md](cloudflare-token-setup.md) instead of inventing placeholders that look real.

Default token permission for deployment:

- `Account -> Cloudflare Pages -> Edit`

### 3. Decide whether to reuse or create a project

- Reuse the same Pages project when updating an existing deliverable.
- Create a new Pages project when:
  - The content is a new standalone deliverable
  - The user wants a distinct public link
  - The naming or ownership boundary should be separate

### 4. Deploy with wrangler

Canonical shape:

```bash
wrangler pages deploy /path/to/site \
  --project-name=<project-name> \
  --branch=main
```

Why `wrangler` is preferred:

- It packages the files correctly.
- It builds the manifest automatically.
- It avoids low-level Pages API errors like missing manifest payloads.

### 5. Report the correct URL

Cloudflare Pages typically exposes two kinds of links:

- Deployment URL: version-specific, usually includes an ID or unique prefix
- Production URL: stable, usually `https://<project-name>.pages.dev`

Always report the Production URL unless the user explicitly asks for the per-deploy preview link.

## Verification Checklist

Run all three checks before reporting success:

1. HTTP status check
2. Content check
3. Asset sanity check

Example checks:

```bash
curl -I https://<project-name>.pages.dev
curl -s https://<project-name>.pages.dev
```

Confirm:

- The status is successful
- The expected title or key body copy exists
- Major styles and assets are loading

## Failure Handling

### `wrangler not found`

- Install `wrangler`
- Re-run the deployment after confirming it is on `PATH`

### `A "manifest" field was expected`

- Do not hand-roll the raw Pages deployment payload
- Use `wrangler pages deploy` instead

### `method_not_allowed`

- Recheck the HTTP method and endpoint path
- If using raw API calls for project creation, verify the account path and method are correct

## Operational Principles

- Prefer local generation plus direct deployment over UI clicking.
- Keep credentials out of repo files and public examples.
- Remove personal names, personal paths, and real account metadata from public samples.
- When updating a page, optimize for stable URLs and repeatable verification.
