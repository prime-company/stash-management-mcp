---
description: Summarize the collection's value, count, and P/L
---

Call the `get_collection_stats` tool from the `stash` MCP server, then
present a concise summary of:

- Total items, broken down by status (in stock / listed / sold)
- Total acquired cost
- Current going-rate sum
- Unrealized P/L (positive or negative)
- Total realized revenue from sold items

Format numbers as USD with two decimal places. If unrealized P/L is
positive, highlight it; if negative, surface the gap.

End with the deep-link citation so the user can open the dashboard.
