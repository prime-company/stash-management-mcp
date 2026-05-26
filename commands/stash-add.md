---
description: Add a new comic to your inventory
---

You are helping the user add a comic to their Stash Management inventory.
The user's input is in `$ARGUMENTS` — it may be unstructured text like
"Amazing Spider-Man #129, NM, $200 acquired".

Steps:
1. Parse `$ARGUMENTS` into the fields supported by `add_inventory_item`:
   item_name, number, publisher, condition_grade, acquired_price,
   going_rate, key_type, autographed, etc. Don't invent fields you can't
   confidently extract.
2. **Always preview first.** Call `add_inventory_item` with `dry_run: true`
   and show the user exactly what will be saved.
3. Ask the user to confirm. Only after confirmation, call again with
   `dry_run: false` to actually create the item.
4. Once created, share the deep-link citation so the user can open the
   item in the web app and fine-tune any fields.

Never call with `dry_run: false` without explicit confirmation from the
user — adding the wrong item is annoying to undo.
