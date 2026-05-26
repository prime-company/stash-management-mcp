---
description: Refresh CovrPrice market values for one or more items
---

The user wants to refresh going_rate from CovrPrice. Their input is in
`$ARGUMENTS` — either a single item id, a search query, or a phrase
like "my keys" / "everything in stock".

Steps:
1. If `$ARGUMENTS` is empty or vague: ask which items to sync. Don't
   sync the whole collection automatically — that burns CovrPrice quota
   and the rate limit is strict.
2. If it's a single uuid, call `sync_price_for_item` directly.
3. If it's a search query, first call `search_inventory` to get the
   matching item ids. Show the user the list and confirm before calling
   `sync_price_for_item` on each.
4. For multi-item sync, call `sync_price_for_item` sequentially — there's
   no bulk variant yet. Stop on any error and report it.

Items marked `covrprice_price_source: "manual"` will be refused — that's
expected; surface it to the user and continue with the rest.
