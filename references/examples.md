# Trigger And Response Examples

## Requests that should trigger this skill

- "把这个 HTML 部署成一个可分享链接"
- "帮我把这个静态页面发到 Cloudflare Pages"
- "我不想点 UI，直接把这个页面上线"
- "更新这个 pages.dev 链接，但不要换地址"
- "把 OpenDesign 产出的 HTML 自动部署掉"

## Expected agent behavior

### Example 1: new deployment

User intent:

> "把这个 landing page 上线成一个公开链接。"

Expected behavior:

- Confirm or create a static site directory
- Choose a project slug
- Deploy to Cloudflare Pages
- Verify production health
- Return the stable production URL

### Example 2: update existing project

User intent:

> "这个项目之前已经有 pages.dev 链接了，直接更新内容。"

Expected behavior:

- Reuse the existing project name
- Redeploy into the same Pages project
- Verify the same production URL now serves the new content
- Return the unchanged production URL

### Example 3: explain the workflow

User intent:

> "OpenDesign 为什么可以不打开网页就直接部署？"

Expected behavior:

- Explain the no-UI chain: local files, CLI or API, then verification
- Emphasize that the deliverable is the final static artifact, not the UI interaction
- Note that the production URL is the stable user-facing result
