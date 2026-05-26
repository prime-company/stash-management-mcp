---
description: Export the workspace inventory to CSV
---

The user wants to export their inventory. Their hints (if any) are in
`$ARGUMENTS` — e.g., "just sold items", "everything sold in April",
"current stock only".

Steps:
1. Parse hints into the `export_inventory_csv` filter arguments:
   - "sold" / "in stock" / "listed" → `status`
   - "in <month> <year>" / "in <year>" → translate to `date_sold_from` and
     `date_sold_to` (full month range or full year range)
   - "since <date>" → `date_sold_from`
2. Call `export_inventory_csv` with the appropriate filters.
3. If `item_count` is 0, suggest broader filters or check the workspace
   has matching items.
4. Otherwise, share the `download_url` and `expires_at` with the user.
   Mention that the link is valid for 24 hours.

Never paraphrase or summarize the CSV contents — the download URL is the
output. The user wants the file, not the data inline.
