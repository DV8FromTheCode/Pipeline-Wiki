# Events Table

## Implemented Events

| Category | Event | Context Variables | Cancellable |
|---|---|---|---|
| Player | `player_join` | `player` | No |
| Player | `player_leave` | `player` | No |
| Player | `player_respawn` | `player` | No |
| Player | `player_chat` | `player`, `message` | Yes |
| Player | `player_command` | `player`, `caller`, `command` | Yes |
| Player | `player_death` | `player`, `damage_source` | No |
| Block | `block_break` | `player`, `block` | Yes |
| Block | `block_place` | `player`, `block` | Yes |
| Entity | `entity_damage` | `entity`, `player`, `damage`, `damage_source` | Yes |
| Item | `item_pickup` | `player`, `item` | Yes |
| Inventory | `inventory_open` | `player`, `inventory` | No |
| Inventory | `inventory_click` | `player`, `inventory`, `slot`, `item`, `click_type` | Yes (cancelled by default for the custom inventory's own slots) |
| Inventory | `inventory_close` | `player`, `inventory` | No |

Inventory handlers must declare `inventory: "<id>"` at the top level of the `.pipe` file; see [Events](../pack-format/events.md#fields).
| Server | `server_start` | — | No |
| Server | `server_stop` | — | No |
| Server | `server_tick` | `tick` | No |
| Custom | Any namespaced name | Caller context plus `fire_event.context` | Yes |

Cancellable world events run before the vanilla operation. For `block_place`, `block` describes the targeted block before placement. `block` is a map: `{{block.block}}`, `{{block.x}}`, `{{block.y}}`, `{{block.z}}`.

For `entity_damage`, `player` is `null` when the damaged entity is not a player; `caller` is the player if there is one, otherwise the entity. For `item_pickup`, `item` is the item ID string. For `inventory_click`, `item` is the inventory item definition (a map — `{{item.custom_data.foo}}` works), `null` when clicking the player's own inventory.

## Planned Events

| Category | Events |
|---|---|
| Player | `player_interact` |
| Entity | `entity_death`, `entity_spawn` |
| Item | `item_drop` |
| World | `world_load`, `world_save`, `world_tick` |

!!! note
    `inventory_drag` is not fired separately. Drag clicks come through as `inventory_click` events, one per dragged slot.
