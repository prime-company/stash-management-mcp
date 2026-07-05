<div align="center">

<img src="./assets/logo.svg" alt="Stash Management" width="120" />

# Stash Management — MCP connector

**Catalog comics at the speed of a scan** — now bring your collection into your AI assistant.

[![Start cataloging free](https://img.shields.io/badge/Start_cataloging-free-EA580C?style=for-the-badge)](https://www.stashmanagement.com/login?intent=signup)
[![Website](https://img.shields.io/badge/stashmanagement.com-111111?style=for-the-badge)](https://www.stashmanagement.com)
[![Protocol: MCP](https://img.shields.io/badge/protocol-MCP-6E56CF?style=for-the-badge)](https://modelcontextprotocol.io)

</div>

---

## 1 · New here? Get a Stash account first

[**Stash Management**](https://www.stashmanagement.com) is comic-book inventory software for **collectors and dealers**. Point your camera at a book and Stash identifies it, returns its current market value, tells you whether to **slab, press, clean, or list** it — and can list it across **twelve marketplaces** including eBay and Whatnot. From your phone, in seconds.

This connector is how you bring that collection into any AI assistant. So step one is an account — **it's free to start**:

### 👉 [Create your free account →](https://www.stashmanagement.com/login?intent=signup)

> AI assistant access is included on paid plans (**Collector** and up). Free accounts can install the connector to try it, but tool calls return an upgrade prompt. See [pricing](https://www.stashmanagement.com/pricing).

Already have an account? Skip to [**Connect your AI app**](#3--connect-your-ai-app).

## 2 · What you can do from your AI assistant

Once connected, just talk to your collection in plain English:

- 🔎 **Find any book** — "Do I own Amazing Spider-Man #300, and what's it worth?"
- 📈 **Know your numbers** — "What's my total collection value and profit this year?"
- 🗝️ **Surface your keys** — "List my major keys, most valuable first."
- 🏷️ **Break it down** — "Which publisher dominates my collection?"
- ✍️ **Keep it current** *(with write access)* — "Add this CGC 9.8 I just bought" or "Mark ASM #129 sold for $2,100."

It's your live Stash data — the same inventory, values, and keys you see on [the web app](https://www.stashmanagement.com), answered conversationally.

## 3 · Connect your AI app

Most clients only need the endpoint URL — they run the sign-in flow for you. You authorize once in your browser, pick which workspace the assistant may see, and you're connected. Access is **read-only by default**.

**MCP endpoint:** `https://www.stashmanagement.com/api/mcp`

<details open>
<summary><b>ChatGPT</b></summary>

Add a custom connector with the URL `https://www.stashmanagement.com/api/mcp`, click **Connect**, and authorize.
</details>

<details>
<summary><b>Claude (Desktop / claude.ai)</b></summary>

Add a custom connector with the same URL — or grab the one-click installer (`.mcpb`) from [stashmanagement.com/connect](https://www.stashmanagement.com/connect).
</details>

<details>
<summary><b>Cursor · VS Code · Windsurf · Cline · other MCP clients</b></summary>

Add the server to your MCP config (see [`examples/mcp.json`](./examples/mcp.json)):

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
</details>

<details>
<summary><b>Claude Code (plugin + slash commands)</b></summary>

This repo doubles as a Claude Code plugin:

```
/plugin marketplace add https://github.com/prime-company/stash-management-mcp
/plugin install stash@stash-management
```

Then run `/mcp` → **stash** → **Authenticate**. Adds the slash commands listed [below](#claude-code-slash-commands).
</details>

<details>
<summary><b>Any other MCP client</b></summary>

Point it at `https://www.stashmanagement.com/api/mcp`. Clients that support MCP OAuth discover the authorization server from the `.well-known` metadata automatically. Headless / terminal clients can use the RFC 8628 device-code flow — enter the one-time code at [stashmanagement.com/activate](https://www.stashmanagement.com/activate).
</details>

## Server reference

| | |
|---|---|
| **MCP endpoint** | `https://www.stashmanagement.com/api/mcp` |
| **Transport** | Streamable HTTP |
| **Auth** | OAuth 2.0 — authorization code + PKCE (browser), or RFC 8628 device code (headless) |
| **Discovery** | `/.well-known/oauth-protected-resource` · `/.well-known/oauth-authorization-server` |
| **Manage / revoke** | [stashmanagement.com/settings/connections](https://www.stashmanagement.com/settings/connections) |

### Tools

Read tools — granted by default (`inventory:read`):

| Tool | Description |
|---|---|
| `get_inventory_summary` | One-shot collection overview (best first call) |
| `search_inventory` | Full-text search across your inventory |
| `get_inventory_item` | Fetch a single item by id |
| `list_recent_items` | Recently added or updated items |
| `list_keys` | Major / minor key issues |
| `list_top_items_by_value` | Top items by value or unrealized gain |
| `get_collection_stats` | Total count, cost, value, profit/loss |
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

Every tool ships a JSON-Schema `outputSchema` and behavior annotations (`readOnlyHint` / `openWorldHint` / `destructiveHint`) per the MCP spec, so assistants understand results and know which calls are safe.

### Claude Code slash commands

Installed via the Claude Code plugin, these wrap the tools (write commands preview with a dry-run and ask before committing):

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

---

<div align="center">

### Don't have a collection in Stash yet?

Scan your first book, get its market value, and start cataloging in seconds.

### 👉 [Start cataloging free →](https://www.stashmanagement.com/login?intent=signup)

[stashmanagement.com](https://www.stashmanagement.com) · [Pricing](https://www.stashmanagement.com/pricing) · [Help](https://www.stashmanagement.com/help/ai-connections)

</div>
