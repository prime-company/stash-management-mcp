# Stash Management — MCP connector

Connect your [Stash Management](https://www.stashmanagement.com) comic-book
inventory to any AI assistant that speaks the [Model Context
Protocol](https://modelcontextprotocol.io). Ask about your collection, look up
values and key issues, and — with your permission — add or update items, all in
plain English from **ChatGPT, Claude, Cursor, or any MCP-compatible client**.

## The server

| | |
|---|---|
| **MCP endpoint** | `https://www.stashmanagement.com/api/mcp` |
| **Transport** | Streamable HTTP |
| **Auth** | OAuth 2.0 — authorization code + PKCE (browser), or RFC 8628 device code (headless) |
| **Discovery** | `/.well-known/oauth-protected-resource` · `/.well-known/oauth-authorization-server` |

You authorize once in your browser and pick which workspace the assistant may
access. Access is **read-only by default**; the write tools require explicitly
granting the `inventory:write` / `actions:trigger` scopes at the consent screen.
Revoke any connection anytime at
<https://www.stashmanagement.com/settings/connections>.

## Connect from your AI app

Most clients only need the endpoint URL — they run the OAuth flow for you.

### ChatGPT
Add a custom connector with the URL
`https://www.stashmanagement.com/api/mcp`, then click Connect and authorize.

### Claude (Desktop / claude.ai)
Add a custom connector with the same URL, or grab the one-click installer
(`.mcpb`) from <https://www.stashmanagement.com/connect>.

### Cursor · VS Code · Windsurf · Cline · other MCP clients
Add the server to your MCP config (e.g. `mcp.json` or the app's MCP settings):

```json
{
  "mcpServers": {
    "stash": {
      "type": "http",
      "url": "https://www.stashmanagement.com/api/mcp"
    }
  }
}
```

A copy is in [`examples/mcp.json`](./examples/mcp.json).

### Claude Code (plugin)
This repo also ships as a Claude Code plugin that bundles the server plus
convenience slash commands (see below):

```
/plugin marketplace add https://github.com/prime-company/stash-management-mcp
/plugin install stash@stash-management
```

Then run `/mcp` → **stash** → **Authenticate** to authorize in your browser.

### Any other MCP client
Point it at `https://www.stashmanagement.com/api/mcp`. Clients that support MCP
OAuth discover the authorization server from the `.well-known` metadata
automatically. Headless / terminal clients can use the device-code flow — enter
the one-time code shown by the client at
<https://www.stashmanagement.com/activate>.

## Tools

Read tools — granted by default (`inventory:read`):

| Tool | Description |
|---|---|
| `get_inventory_summary` | One-shot collection overview (best first call) |
| `search_inventory` | Full-text search across your inventory |
| `get_inventory_item` | Fetch a single item by id |
| `list_recent_items` | Recently added or updated items |
| `list_keys` | Major / minor key issues |
| `list_top_items_by_value` | Top items by value or unrealized gain |
| `get_collection_stats` | Total count, cost, value, P/L |
| `get_publisher_breakdown` | Items grouped by publisher |
| `get_recent_activity` | Recent actions in your workspace |

Write / action tools — require consent (`inventory:write`, `actions:trigger`):

| Tool | Scope | Description |
|---|---|---|
| `add_inventory_item` | `inventory:write` | Create a new comic |
| `update_inventory_item` | `inventory:write` | Update an item by id |
| `mark_item_sold` | `inventory:write` | Record a sale |
| `sync_price_for_item` | `inventory:write` | Refresh market price from external sources |
| `export_inventory_csv` | `actions:trigger` | Export inventory to a CSV (signed URL) |
| `bulk_enrich` | `actions:trigger` | Enrich items with covers / metadata |

Every tool ships a JSON-Schema `outputSchema` and behavior annotations
(`readOnlyHint` / `openWorldHint` / `destructiveHint`) per the MCP spec, so
assistants understand results and know which calls are safe.

## Claude Code slash commands

When installed as the Claude Code plugin, these convenience commands wrap the
tools (write commands preview with a dry-run and ask before committing):

| Command | What it does |
|---|---|
| `/stash-summary` | One-shot collection overview |
| `/stash-search <query>` | Search your inventory |
| `/stash-stats` | Count, value, and profit/loss |
| `/stash-keys [major]` | List your key issues |
| `/stash-add <description>` | Add a comic (dry-run, then confirm) |
| `/stash-sold <id-or-description> <price>` | Record a sale (previews net gain/loss) |
| `/stash-export [filters]` | Export to CSV (natural-language filters) |
| `/stash-recent [limit]` | Most recently added / updated items |
| `/stash-publishers` | Breakdown by publisher |
| `/stash-sync-prices <id-or-query>` | Refresh market price for an item |

## Requirements

A Stash Management account. AI access is included on paid plans (Collector and
up). Free accounts can install the connector, but tool calls return an upgrade
prompt.

## Development

The connector/plugin source lives in the `stash-management` monorepo under
`scripts/stash-plugin/` and is mirrored to this repo. To publish an update, sync
that folder to this repo's root over HTTPS and push `main`.
