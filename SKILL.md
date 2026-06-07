---
name: open-design-cloudflare-pages-deploy
description: Generate or refine static HTML and deploy it to Cloudflare Pages using an OpenDesign-style no-UI workflow. Use when the user wants a local HTML/CSS/JS output turned into a shareable `*.pages.dev` link, wants to update an existing Pages project while keeping the public URL stable, or asks how to automate OpenDesign-style deployment through files, CLI, and API instead of clicking through a web UI.
---

# OpenDesign Cloudflare Pages Deploy

Deploy local static output to a stable public Cloudflare Pages URL without relying on a browser UI.

## Quick Start

1. Confirm the deliverable is a static site folder with an `index.html` entrypoint.
2. Prefer generating or editing the final HTML/CSS/JS locally instead of waiting on a design UI.
3. Read [references/workflow.md](references/workflow.md) for the deploy sequence.
4. Deploy to Cloudflare Pages and report only the Production URL, never a one-off deployment URL.
5. Run post-deploy verification before declaring success.

## Default Workflow

### 1. Prepare the artifact

- Ensure the output is a static directory, usually containing `index.html` plus optional assets.
- If the user starts from a prompt or document, generate the final HTML directly unless a separate OpenDesign generation step is explicitly required.
- Keep the deploy target minimal; remove temp files and editor artifacts.

### 2. Prepare the environment

- Confirm Cloudflare credentials are available through environment variables or a local config file.
- Use `wrangler` for deployment because it handles manifest creation automatically.
- If `wrangler` is missing, install it before proceeding.

### 3. Choose project strategy

- Reuse an existing Pages project when the user wants to keep the same public link.
- Create a new Pages project when this is a new deliverable or should have its own public URL.
- Normalize the project name to lowercase hyphen-case before deploy.

### 4. Deploy

- Run a Pages deploy against the static directory.
- Treat the non-ID `https://<project-name>.pages.dev` address as the canonical result.
- Do not send the user a deployment-specific preview URL unless they explicitly ask for it.

### 5. Verify

- Check the production URL returns a successful HTTP status.
- Fetch the page content and confirm title, key text, and major assets are present.
- If verification fails, debug first and only report the URL after the production page is healthy.

## Operating Rules

- Default to a no-UI path: local files, CLI, API, then verification.
- Keep secrets out of logs and committed files.
- Never hardcode or commit real API tokens, account IDs, local home paths, or user-specific machine details.
- Prefer production URL stability over creating fresh random links for small updates.
- If the user mentions OpenDesign, interpret that as workflow context, not a requirement to open OpenDesign UI.

## When To Read References

- Read [references/workflow.md](references/workflow.md) for the exact deploy sequence, validation checklist, and failure handling.
- Read [references/examples.md](references/examples.md) when you need trigger examples, naming patterns, or response patterns.
- Read [references/cloudflare-token-setup.md](references/cloudflare-token-setup.md) when the user needs to apply for or configure a Cloudflare API token safely.
- Read [references/github-publishing.md](references/github-publishing.md) when the user wants to package, document, or publish this skill as a GitHub project.
