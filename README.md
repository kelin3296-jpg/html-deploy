# Pages Deploy

[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
![Skill](https://img.shields.io/badge/skill-SKILL.md-blue.svg)
![Cloudflare Pages](https://img.shields.io/badge/cloudflare-pages-f38020.svg)
![Agent Compatible](https://img.shields.io/badge/agent--compatible-Codex%20%7C%20Claude%20Code%20%7C%20Hermes%20%7C%20OpenClaw-111827.svg)

An agent-ready deployment skill for turning local static HTML into a stable public Cloudflare Pages URL.

Pages Deploy is built for agents that should work from local files, CLI, and verification instead of clicking through a web app.

Compatible targets:

- Codex
- Claude Code
- Hermes
- OpenClaw

## Quick Start

1. Install the skill into your local agent skills directory.

```bash
~/.codex/skills/pages-deploy
```

2. Trigger it with a direct prompt.

```text
Use $pages-deploy to publish the HTML in ./dist and give me the stable production URL.
```

3. Expect the agent to deploy with Cloudflare Pages, verify the live page, and return a stable URL like:

```text
https://your-project.pages.dev
```

## Value

Pages Deploy gives an agent one clean job:

- take a local static site
- publish it to Cloudflare Pages
- keep the public URL stable when needed
- verify the live result before reporting success

## Features

- Publishes a local HTML/CSS/JS folder to Cloudflare Pages
- Reuses an existing Pages project when the public URL should stay unchanged
- Explains and automates an OpenDesign-style no-UI deployment flow
- Guides users through Cloudflare API Token setup without exposing secrets

## Compatibility

This repository is packaged as a `SKILL.md`-based skill, so the most direct drop-in experience is for agent systems that support skill folders and markdown instructions.

In practice:

- Codex can use the repository directly as a local skill
- Claude Code, Hermes, and OpenClaw can reuse the same instructions, references, prompts, and workflow design
- if a target framework uses a different plugin or skill manifest format, keep `SKILL.md` as the source of truth and adapt the wrapper only

## Use Cases

Use this skill when you want an agent to handle requests like:

- "把这个 HTML 部署成一个可分享链接"
- "帮我把这个静态页面上线到 Cloudflare Pages"
- "不要打开 UI，直接把页面发出去"
- "更新这个 `pages.dev` 项目，但不要换链接"

## Install

### Codex

Copy or symlink this repository into your Codex skills directory.

Common location:

```bash
~/.codex/skills/pages-deploy
```

### Other agent runtimes

For Claude Code, Hermes, and OpenClaw:

- place this repository in the runtime's skill, prompt, or agent-capability directory
- use `SKILL.md` as the primary instruction file
- keep the `references/` folder next to it so the agent can load deeper guidance on demand
- adapt only the runtime-specific wrapper if needed; do not fork the workflow content unless behavior truly differs

## Agent install prompt

If you want another agent to install this skill for you, send it this prompt:

```text
Install the skill from https://github.com/kelin3296-jpg/pages-deploy into my local agent skills directory. Clone the repository, keep SKILL.md and the references folder together, verify that SKILL.md and agents/openai.yaml exist, and confirm the skill can be discovered or referenced locally by my agent runtime.
```

If the target agent supports symlinks and you prefer that setup, replace the clone step with a symlink into the same destination.

## How to trigger the skill

After installation, ask the agent for the task directly, or invoke the skill by name if the runtime supports explicit skill calls.

Examples:

```text
Use $pages-deploy to publish the HTML in ./dist and give me the stable production URL.
```

```text
Use $pages-deploy to update my existing Cloudflare Pages project without changing the public link.
```

## Example conversation

```text
User:
Use $pages-deploy to publish the static site in ./site.
Project name should be demo-landing-pages.
If Cloudflare credentials are missing, tell me exactly what I need to apply for.

Agent:
I’ll deploy the local static site in ./site to Cloudflare Pages, reuse the project name demo-landing-pages, and return only the stable production URL after verification. If credentials are missing, I’ll stop and tell you how to create the required Cloudflare API Token and Account ID setup.
```

```text
User:
Use $pages-deploy to redeploy this existing project, keep the same pages.dev URL, and verify the new copy is live.

Agent:
I’ll redeploy into the existing Pages project so the production URL stays unchanged, then check the live page before reporting completion.
```

## Runtime requirements

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

This skill is opinionated about three things:

1. Prefer local files and CLI over clicking through a deployment UI
2. Return the stable production URL, not a one-off deployment preview link
3. Verify the page after deployment before declaring success

## Repository structure

```text
 pages-deploy/
├── SKILL.md
├── README.md
├── LICENSE
├── .gitignore
├── agents/
│   └── openai.yaml
└── references/
    ├── cloudflare-token-setup.md
    ├── examples.md
    ├── github-publishing.md
    └── workflow.md
```

## Reference files

- [SKILL.md](SKILL.md): agent-facing instructions
- [references/workflow.md](references/workflow.md): deployment flow and validation
- [references/cloudflare-token-setup.md](references/cloudflare-token-setup.md): token application and safe local setup
- [references/examples.md](references/examples.md): request and response examples

## Positioning

This project is not a Cloudflare Pages wrapper for one specific coding agent. It is a reusable deployment workflow package for AI agents that generate or edit HTML locally and then need to publish it with a safe, repeatable, stable-link workflow.

## License

MIT
