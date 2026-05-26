---
description: Break down the collection by publisher
---

Call the `get_publisher_breakdown` MCP tool from the `stash` server.

Present the result as a sorted list: publisher name, count of items,
total acquired cost, total going-rate. Highlight the top 3 publishers
by count.

Mention the percentage of the collection each top publisher represents
based on item counts (publisher.count / sum(count)).

If the user asks "which publisher is most valuable?", sort by
total_going_rate instead of count.
