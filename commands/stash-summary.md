---
description: One-shot overview of the user's whole collection
---

Call the `get_inventory_summary` MCP tool from the `stash` server. This
is the right tool when the user asks broad questions like:

- "What's in my collection?"
- "Give me an overview."
- "How am I doing?"
- "Summarize my stash."

Don't call get_collection_stats + list_top_items_by_value +
get_publisher_breakdown + get_recent_activity separately for this kind
of prompt — one summary call returns all four datasets in a single
round-trip.

Present the result in this order:

1. **Totals** — item count, total acquired cost, current going-rate sum,
   unrealized P/L (highlight if positive)
2. **Top items** — the 5 most valuable items by going rate, with deep links
3. **Top publishers** — which publishers dominate, by count
4. **Recent activity** — the 5 most recent actions in the workspace

End with the dashboard citation link so the user can dive deeper.
