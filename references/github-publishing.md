# GitHub Publishing Notes

Use this reference when packaging the skill as a public repository.

## Repository goals

- Make the skill easy to discover and install
- Keep secrets and personal machine paths out of the repo
- Separate agent-facing instructions from human-facing README content

## Recommended repo contents

- `SKILL.md`
- `agents/openai.yaml`
- `references/`
- `README.md`
- `LICENSE`
- `.gitignore`

## README should cover

- What the skill does
- Which requests should trigger it
- How to install it into a Codex or compatible skills directory
- What workflow assumptions it makes
- What credentials are needed at runtime

## Do not commit

- Real Cloudflare API tokens
- Real account IDs tied to personal infrastructure unless intentionally public
- Machine-specific config files from the home directory
- Generated logs containing credential material

## Suggested installation snippet

Tell users to copy or symlink the repository into their skill search path, for example:

- `~/.codex/skills/`
- another configured skills directory

## Suggested open source framing

Position the project as:

- A no-UI deployment skill for static HTML
- Inspired by an OpenDesign workflow
- Focused on Cloudflare Pages production URL stability and automated verification
