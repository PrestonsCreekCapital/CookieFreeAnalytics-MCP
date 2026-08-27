# Cookie Free Analytics MCP

Remote [Model Context Protocol](https://modelcontextprotocol.io) server for [Cookie Free Analytics](https://www.cookiefreeanalytics.com): cookieless web analytics, EU-hosted, GDPR.

This repository is the public listing (Official MCP Registry, Glama, and install copy). The server itself runs at the product origin. There is nothing to `npm install`.

Operator: Prestons Creek Capital. Public contact: hello@cookiefreeanalytics.com.

| | |
|---|---|
| Product | [cookiefreeanalytics.com](https://www.cookiefreeanalytics.com) |
| MCP URL | `https://www.cookiefreeanalytics.com/mcp` |
| Transport | Streamable HTTP |
| Auth | OAuth 2.1 (RFC 9728 + PKCE). Optional hashed bearer from Account. |
| Registry name | `io.github.PrestonsCreekCapital/cookie-free-analytics` |
| Docs | [docs/api](https://www.cookiefreeanalytics.com/docs/api) |

Starter and Growth. Hobby is the dashboard only.

The tools return the same aggregates as the dashboard. They do not return visitor hashes, raw IPs, or a user graph. They do not write pageviews, change settings, or send mail.

## Install

Paste this URL as a custom connector:

```
https://www.cookiefreeanalytics.com/mcp
```

The client starts OAuth 2.1 on cookiefreeanalytics.com (protected-resource metadata, PKCE S256, dynamic client registration). You sign in to Cookie Free Analytics. A Google login on the website is not an MCP token.

### Claude

Claude.ai / Claude Desktop: **Settings → Connectors → Add custom connector**. URL above. No Client ID or secret.

Claude Code:

```bash
claude mcp add --transport http cookie-free-analytics https://www.cookiefreeanalytics.com/mcp
```

### Cursor

`~/.cursor/mcp.json` (or project `.cursor/mcp.json`):

```json
{
  "mcpServers": {
    "cookie-free-analytics": {
      "url": "https://www.cookiefreeanalytics.com/mcp"
    }
  }
}
```

### VS Code

`.vscode/mcp.json` or user MCP config. Command Palette: **MCP: Add Server**.

```json
{
  "servers": {
    "cookie-free-analytics": {
      "type": "http",
      "url": "https://www.cookiefreeanalytics.com/mcp"
    }
  }
}
```

VS Code opens a browser for OAuth on first connect.

### Grok

[grok.com/connectors](https://grok.com/connectors) → custom connector → the same URL. There is no Grok public catalog entry to search for.

## OAuth flow

1. The client `GET`s RFC 9728 metadata for `https://www.cookiefreeanalytics.com/mcp`.
2. PKCE S256 + dynamic client registration. You do not paste a Client ID or Client Secret.
3. Browser sign-in on cookiefreeanalytics.com. Starter or Growth.
4. The client calls `POST /mcp` with the access token.

curl and scripts may still send `Authorization: Bearer` with a hashed token from **Plan → Account → Read API** (`cfa_live_…`, shown once). Do not commit that token.

## Tools (read-only)

| Tool | Returns |
|---|---|
| `list_sites` | Sites the account owns |
| `get_overview` | Visitors, visits, pageviews, bounce, duration, live, previous period |
| `get_pages` | Top pages |
| `get_sources` | Top sources |
| `get_live` | Last pageview per visitor in five minutes (no heartbeat) |
| `get_funnels` | Linear same-day funnels (Growth) |

`range` is `today`, `7d`, `30d`, `90d`, or `YYYY-MM-DD..YYYY-MM-DD`. `f` is the same filter string the dashboard puts in the URL.

## Docs

- Product: https://www.cookiefreeanalytics.com
- Read API + MCP: https://www.cookiefreeanalytics.com/docs/api
- How-to: https://www.cookiefreeanalytics.com/blog/cookieless-analytics-mcp
- Privacy: https://www.cookiefreeanalytics.com/privacy
- Support: hello@cookiefreeanalytics.com

## License

MIT. Operator: Prestons Creek Capital.
