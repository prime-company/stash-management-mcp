---
description: Show recently added or updated inventory items
---

Call the `list_recent_items` MCP tool from the `stash` server. Default
limit is 10; the user can ask for more (cap at 50).

Present each item with: title + issue number, publisher, status, the
last-updated timestamp, and the deep-link citation.

If `$ARGUMENTS` is a number, use that as the limit.
