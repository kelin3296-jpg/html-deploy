# Testing

This repository has two test levels:

- static validation, which does not need Cloudflare credentials
- live deployment, which needs a real Cloudflare account and API token

## Static Validation

Run this before publishing changes to the skill.

### 1. Validate the skill file

Use the skill validation tool available in your local agent environment.

For Codex, run:

```bash
python3 ~/.codex/skills/.system/skill-creator/scripts/quick_validate.py .
```

Expected result:

```text
Skill is valid!
```

### 2. Check required documentation

Confirm these files exist:

```text
SKILL.md
README.md
agents/openai.yaml
references/workflow.md
references/cloudflare-token-setup.md
references/examples.md
```

Confirm the docs include:

- install location
- trigger prompt
- Cloudflare API Token application link
- recommended token scope
- production URL reporting rule

### 3. Check for sensitive data

Run a basic local scan before committing:

```bash
rg -n "cf[a-zA-Z0-9_-]{20,}|/Users/|Account ID:|account_id" . -g '!TESTING.md'
rg -n -P 'CLOUDFLARE_API_TOKEN=(?!"your-real-token-here"|"\.\.\.")' . -g '!TESTING.md'
```

Expected result:

- no real Cloudflare API token
- no real account ID
- no personal local machine path

Placeholders such as `CLOUDFLARE_API_TOKEN`, `CLOUDFLARE_ACCOUNT_ID`, `your-real-token-here`, and `...` are acceptable.

## Live Deployment Test

Run this only with a real Cloudflare account and a token scoped for deployment.

### 1. Create a minimal HTML site

```bash
mkdir -p /tmp/html-deploy-test
cat > /tmp/html-deploy-test/index.html <<'HTML'
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>HTML Deploy Test</title>
  </head>
  <body>
    <h1>HTML Deploy Test</h1>
    <p>Deployment verification marker: html-deploy-live-test</p>
  </body>
</html>
HTML
```

### 2. Configure Cloudflare credentials

Create a Cloudflare API token at:

```text
https://dash.cloudflare.com/profile/api-tokens
```

Recommended custom token permission:

```text
Account -> Cloudflare Pages -> Edit
```

Then expose credentials locally:

```bash
export CLOUDFLARE_API_TOKEN="your-real-token-here"
export CLOUDFLARE_ACCOUNT_ID="your-account-id-here"
```

Do not commit these values.

### 3. Deploy with wrangler

```bash
wrangler pages deploy /tmp/html-deploy-test \
  --project-name=html-deploy-live-test \
  --branch=main
```

### 4. Verify the production URL

```bash
curl -I https://html-deploy-live-test.pages.dev
curl -s https://html-deploy-live-test.pages.dev | rg "html-deploy-live-test"
```

Expected result:

- the production URL returns a successful HTTP status
- the verification marker exists in the HTML response

### 5. Cleanup decision

Keep the test project if you want a permanent smoke-test target.

Delete it from Cloudflare Pages if it was only created for one-time verification.

## Current Status

At the time this document was added:

- static skill validation had passed
- GitHub repository publishing had been verified
- live Cloudflare deployment had not been run because it requires real Cloudflare credentials
