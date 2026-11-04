# Documentation Site Deployment

The documentation at [mcp-notify.modelcontextprotocol.name](https://mcp-notify.modelcontextprotocol.name) is a MkDocs Material site hosted on Cloudflare Pages.

| Item | Value |
|------|-------|
| Host | Cloudflare Pages, project `mcp-notify-docs` |
| Production URL | `https://mcp-notify.modelcontextprotocol.name` |
| Security headers | `docs/_headers` (copied into the build) |
| DNS | Proxied CNAME in the `modelcontextprotocol.name` Cloudflare zone, pointing to `mcp-notify-docs.pages.dev` |

## Build

```bash
pip install 'mkdocs-material[imaging]' mkdocs-minify-plugin \
  mkdocs-git-revision-date-localized-plugin mkdocs-git-committers-plugin-2
# git-committers calls the GitHub API once per page; skip it for fast local builds
GIT_COMMITTERS_ENABLED=false mkdocs build            # output: site/
```

The social cards plugin needs the Cairo system library (`apt-get install libcairo2`).
The git plugins need full history, so run `git fetch --unshallow` in a shallow clone.

## Deploy

Export `CLOUDFLARE_API_TOKEN` and `CLOUDFLARE_ACCOUNT_ID`, then run from the repository root:

```bash
npx wrangler@4 pages deploy site --project-name=mcp-notify-docs --branch=main
```

Deploys are manual. GitHub Actions is not used for this site.

## Verify

```bash
curl -sI https://mcp-notify.modelcontextprotocol.name | head -1   # HTTP/2 200
```

## Rollback

In the Cloudflare dashboard, open **Workers & Pages → mcp-notify-docs → Deployments**.
Pick an earlier production deployment and select **Rollback to this deployment**.
