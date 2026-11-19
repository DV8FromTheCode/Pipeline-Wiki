# Inventories

Pipeline can create server-side inventory GUIs that work with unmodified vanilla clients.

## Example

```yaml
# data/my_pack/inventories/shop.pipe
id: "my_namespace:shop"
title: "&6Shop"
type: chest
rows: 3

items:
  - slot: 0
    item: diamond
    amount: 1
    name: "&bBuy Diamond"
    lore:
      - "&7Price: &a100 coins"
    custom_data:
      shop_item: "buy_diamond"

  - slot: 26
    item: barrier
    name: "&cClose"
    custom_data:
      action: "close"
```

## Container Types

| Type | Slots | Notes |
|---|---|---|
| `chest` | 9-54 (1-6 rows) | Standard chest UI. |
| `hopper` | 5 | Currently rendered as a 1-row chest. |
| `dropper` | 9 | Currently rendered as a 1-row chest. |

## Opening and Closing

From a command or event:

```yaml
- open_inventory: "my_namespace:shop"
  to: "{{caller}}"

- close_inventory: "{{player.name}}"
```

## Handling Clicks

```yaml
# data/my_pack/events/on_inventory_click.pipe
on: inventory_click
inventory: "my_namespace:shop"

actions:
  - if: "{{item.custom_data.shop_item == 'buy_diamond'}}"
    then:
      - storage.increment: "player_data.{{player.uuid}}.balance"
        amount: -100
      - command: "give {{player.name}} diamond 1"
      - tell: "{{player.name}}"
        message: "&aPurchased!"
    else:
      - cancel_event: true
```

## Notes

- Dragging, shift-clicking, and item pickup/drop inside custom inventories are cancelled by default.
- The player's own inventory is never touched.
