---
description: List the user's key issues (major / minor)
---

Call the `list_keys` MCP tool from the `stash` server. If `$ARGUMENTS`
mentions "major", set `major_only: true`. Otherwise list both major and
minor keys.

Present each key with: title + issue number, publisher, key type (major
or minor), going rate, and the deep-link citation.

Sort by going rate descending (the tool already does this) and report up
to 20 by default. If the user asks for "all keys", request the full set
with `limit: 100`.
