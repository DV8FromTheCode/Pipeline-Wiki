# Example Pack Tour

The `example-pack/` folder in the main Pipeline repo is a reference pack. Copy it into `run/world/datapacks/` to see pretty much every implemented feature in action.

## What's Inside

### Commands (`data/hello/commands/`)

- `greet.pipe` — basic greeting command
- `shop.pipe` — opens the example shop GUI
- `clear_data.pipe` — wipes stored player data
- `ping_network.pipe` — shows how `network.send` looks
- `request_http.pipe` and `http_demo.pipe` — submit an approval request, then perform a bounded HTTP GET
- `custom_event.pipe` — fires a namespaced event with custom context

### Events (`data/hello/events/`)

Handlers for join/leave/respawn, chat, commands, death, cancellable block/entity/item interactions, inventory open/click/close, server lifecycle, and `hello:announcement`, a custom event.

### Schedules (`data/hello/schedules/`)

- `daily_reset.pipe` — daily reset at a set time
- `interval_announce.pipe` — recurring announcement
- `tick_welcome.pipe` — per-tick example

### Storage (`data/hello/storage/`)

- `player_data.pipe` — per-player schema
- `global_config.pipe` — global schema
- `world_spawn.pipe` — per-world schema

### Permissions (`data/hello/permissions/`)

Example permissions with OP and default levels.

### Registry (`data/hello/registry/`)

- `items.pipe` — custom sword
- `blocks.pipe` — custom block proxy

### Inventories (`data/hello/inventories/`)

- `shop.pipe` — chest-style shop GUI

### Network (`data/hello/network/`)

- `custom_ui.pipe` — example packet schema

## Try It Out

1. Copy `example-pack/` into `run/world/datapacks/`.
2. Run `/pipeline reload` if the server is already up.
3. Join and run `/shop`, `/greet`, and `/ping_network`.
4. Check `run/logs/latest.log` for `[Pipeline]` output.
