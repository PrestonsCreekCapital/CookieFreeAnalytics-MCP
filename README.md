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
| Registry name | `com.cookiefreeanalytics/mcp` |
| Docs | [docs/api](https://www.cookiefreeanalytics.com/docs/api) |

See your website visitors without a cookie banner, from your AI app.

- **Hobby (free):** 5 tools: `get_traffic_pulse`, `get_top_pages`, `get_watch_status`, `add_website`, `get_tracking_script`, with small daily limits.
- **Starter and Growth:** all 13 tools.

The read tools return the same totals as the dashboard. They never return visitor hashes, raw IPs, or a user graph. The only write is `add_website` (needs the `sites:write` permission you approve on the consent screen). Nothing deletes data, changes settings, or sends mail.

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
3. Browser sign-in on cookiefreeanalytics.com (any plan, Hobby included), then **Allow** on the consent screen. Scopes: `analytics:read`, `sites:write`.
4. The client calls `POST /mcp` with the access token.

curl and scripts may still send `Authorization: Bearer` with a hashed token from **Plan → Account → Read API** (`cfa_live_…`, shown once). Do not commit that token.

## Tools

Free on Hobby:

| Tool | Does | Hobby daily limit |
|---|---|---|
| `get_traffic_pulse` | Visitors, visits and pageviews for today, yesterday, the last 7 days and the 7 days before | 20 |
| `get_top_pages` | Top 5 pages over the last 7 days | 10 |
| `get_watch_status` | Are the watched pages up? | 10 |
| `add_website` | Adds a website to your account (checks the address, skips duplicates, respects the plan's site limit) and returns its id | 10 |
| `get_tracking_script` | The one line to paste, simple steps for WordPress, Wix, Squarespace, Shopify, Webflow, plain HTML and Google Tag Manager, a note to send your web person, and whether visits are arriving yet | 30 |

Paid plans add:

| Tool | Returns |
|---|---|
| `list_sites` | Sites the account owns |
| `get_overview` | Visitors, visits, pageviews, bounce, duration, live, previous period |
| `get_pages` | Top pages |
| `get_sources` | Top sources |
| `get_live` | Last pageview per visitor in five minutes (no heartbeat) |
| `get_funnels` | Linear same-day funnels (Growth) |
| `get_watch` | Uptime, TTFB, TLS, current state (Starter and Growth) |
| `get_watch_incidents` | Open and recent Watch incidents (Starter and Growth) |

`range` is `today`, `7d`, `30d`, `90d`, or `YYYY-MM-DD..YYYY-MM-DD`. `f` is the same filter string the dashboard puts in the URL.

The server accepts protocol `2026-07-28` and the older handshake (`2025-11-25` back to `2024-11-05`). There is no package to run.

## Docs

- Product: https://www.cookiefreeanalytics.com
- Read API + MCP: https://www.cookiefreeanalytics.com/docs/api
- How-to: https://www.cookiefreeanalytics.com/blog/cookieless-analytics-mcp
- Privacy: https://www.cookiefreeanalytics.com/privacy
- Support: hello@cookiefreeanalytics.com

## License

MIT. Operator: Prestons Creek Capital.
