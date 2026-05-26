---
description: Record a sale on an inventory item
---

The user wants to record a sale. Their input is in `$ARGUMENTS` — usually
either an item id or a description (e.g., "Spider-Man 129 for 240").

Steps:
1. If the user provided a description rather than an id, call
   `search_inventory` first to find the matching item. Confirm with the
   user that you picked the right one.
2. Call `mark_item_sold` with `dry_run: true` to preview. The dry-run
   response includes the computed net gain/loss — share that with the
   user so they can sanity-check the price.
3. After explicit confirmation, call again with `dry_run: false`.
4. Share the deep-link citation so the user can view the updated item.

If the user mentions a date, parse it to YYYY-MM-DD and pass as
`date_sold`. Otherwise omit it — the tool defaults to today.
