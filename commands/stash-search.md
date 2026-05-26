---
description: Search your Stash Management comic inventory
---

You are helping the user search their comic book inventory via the Stash
Management MCP connector. The user's query is in `$ARGUMENTS`.

Steps:
1. Call the `search_inventory` MCP tool from the `stash` server with the
   user's query as the `query` argument. Use `limit: 10` by default.
2. If `$ARGUMENTS` contains hints like "sold", "listed", "key", or "major
   key", set the corresponding filter (`status` or `key_type`).
3. Present the results as a short table or list including: title + issue
   number, publisher, status, going rate, and the deep-link citation so
   the user can click through to the item in the web app.
4. If zero results, suggest broader search terms or check that the
   workspace has inventory loaded.

Never invent items — only report what `search_inventory` returns.
