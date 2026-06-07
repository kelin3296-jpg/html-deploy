# OpenDesign Cloudflare Pages Deploy

A reusable Codex skill for turning local static HTML into a shareable Cloudflare Pages link without opening a deployment UI.

It is designed around an OpenDesign-style workflow:

- generate or refine the final HTML locally
- deploy with `wrangler`
- return the stable production URL
- verify the live page before reporting success

## What this skill is for

Use this skill when you want an agent to:

- publish a local HTML/CSS/JS folder to Cloudflare Pages
- update an existing `pages.dev` project without changing the public URL
- explain or automate a no-UI deployment workflow for static sites

## Repository structure

```text
open-design-cloudflare-pages-deploy/
├── SKILL.md
├── README.md
├── LICENSE
├── .gitignore
├── agents/
│   └── openai.yaml
└── references/
    ├── examples.md
    ├── github-publishing.md
    └── workflow.md
```

## Install

Copy or symlink this folder into your Codex skills directory.

Common location:

```bash
~/.codex/skills/open-design-cloudflare-pages-deploy
```

## Agent install prompt

If you want another agent to install this skill for you, send it this prompt:

```text
Install the skill from https://github.com/kelin3296-jpg/open-design-cloudflare-pages-deploy into my Codex skills directory. Clone the repository into ~/.codex/skills/open-design-cloudflare-pages-deploy, verify that SKILL.md and agents/openai.yaml exist, and confirm the skill can be discovered locally.
```

If the target agent supports local symlinks and you prefer that setup, replace the clone step with a symlink into the same destination.

## Runtime expectations

This skill assumes:

- you have a local static site directory with `index.html`
- `wrangler` is available, or can be installed
- Cloudflare credentials are available at runtime through environment variables or a protected local config

See [references/cloudflare-token-setup.md](references/cloudflare-token-setup.md) for a safe token application flow.

Common environment variables:

```bash
export CLOUDFLARE_API_TOKEN="..."
export CLOUDFLARE_ACCOUNT_ID="..."
```

Do not commit real credential values, `.env` files, or machine-specific config files into the repository.

## Core behavior

The skill is opinionated about three things:

1. Prefer local files and CLI over clicking through a web UI
2. Return the stable production URL, not a one-off deployment preview link
3. Verify the page after deployment before declaring success

## License

MIT
