# Stash Management (Claude Code plugin)

Adds the Stash Management MCP server to Claude Code so you can query your
comic inventory directly: search keys, get collection stats, recap recent
activity — all from the chat.

## Install

```
/plugin marketplace add prime-company/stash-management-mcp
/plugin install stash
```

On first tool call, the plugin uses the OAuth 2.0 device-code flow:

1. Claude Code shows you a one-time code like `ABCD-EFGH`
2. Visit <https://stashmanagement.com/activate> in any browser
3. Type the code, log in, pick a workspace, click Allow
4. Claude Code automatically picks up the connection — you stay in the terminal

The two-step pattern (code in terminal + code typed into browser) means
nobody can phish your authorization — only the session that initiated the
request can pick up the resulting tokens.

Tokens are stored by Claude Code locally. Revoke anytime at
<https://stashmanagement.com/settings/connections>.

## What it adds

### Slash commands

| Command | What it does |
|---|---|
| `/stash-summary` | One-shot collection overview (best first command) |
| `/stash-search <query>` | Search your inventory |
| `/stash-stats` | Summarize collection count, value, P/L |
| `/stash-keys [major]` | List your key issues |
| `/stash-add <description>` | Add a comic — previews with dry-run, asks to confirm |
| `/stash-sold <id-or-description> <price>` | Record a sale — previews net gain/loss first |
| `/stash-export [filters]` | Export inventory to CSV (24h signed URL). Natural-language filters supported. |
| `/stash-recent [limit]` | Most recently added or updated items |
| `/stash-publishers` | Collection breakdown by publisher |
| `/stash-sync-prices <id-or-query>` | Refresh going_rate from CovrPrice (refuses to sync the whole collection at once) |

### MCP tools (callable by name in any prompt)

| Tool | Description |
|---|---|
| `search_inventory` | Full-text search across your inventory |
| `get_inventory_item` | Fetch a single item by id |
| `list_recent_items` | Recently added or updated items |
| `list_keys` | Major / minor key issues |
| `list_top_items_by_value` | Top items by going rate or unrealized gain |
| `get_collection_stats` | Total count, cost, value, P/L |
| `get_publisher_breakdown` | Items grouped by publisher |
| `get_recent_activity` | Recent actions in your workspace |
| `add_inventory_item` | Create a new comic (dry-run by default in slash commands) |
| `update_inventory_item` | Partial update by id |
| `mark_item_sold` | Record a sale; dry-run shows net gain/loss |
| `sync_price_for_item` | Refresh going_rate from CovrPrice |
| `export_inventory_csv` | Download a CSV (signed URL, 24h expiry) |

## Requirements

- Claude Code 1.0+
- A Stash Management account on the Collector plan or higher

## Publishing

This folder is the source of truth in the `stash-management` monorepo. To
publish, mirror it to its own repo at `stashmanagement/stash-plugin` (the
marketplace looks up plugins by `owner/repo`).

```
# from the monorepo root
git subtree push --prefix=scripts/stash-plugin git@github.com:prime-company/stash-management-mcp.git main
```
