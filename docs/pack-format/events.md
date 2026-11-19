# Events

Hook into server and gameplay events with `.pipe` files in `data/<namespace>/events/`.

## Example

```yaml
# data/my_pack/events/on_player_join.pipe
on: player_join
priority: 10

actions:
  - tell: "@a"
    message: "&e{{player.display_name}} has joined!"

  - if: "{{player.has_permission('my_namespace.admin')}}"
    then:
      - command: "gamemode creative {{player.name}}"
    else:
      - command: "gamemode survival {{player.name}}"
```

## Fields

| Field | Description |
|---|---|
| `on` | Event name to listen for. |
| `priority` | Lower numbers run first. Default is `10`. |
| `actions` | What to do when the event fires. `execute` is accepted as an alias. |
| `inventory` | **Required** for `inventory_open`/`inventory_click`/`inventory_close`. The handler only fires for this exact inventory ID (e.g. `my_namespace:shop`); a handler without it never runs. |

## Context Variables

Variables depend on the event. Common ones:

| Variable | Description |
|---|---|
| `{{player}}` | The player involved. |
| `{{caller}}` | Command sender (player or console). |
| `{{pack}}` | Name of the pack that owns the running file. |
| `{{now}}` | Current timestamp. |

Event-specific variables like `{{slot}}` and `{{item}}` for inventory events are listed in the [Events Table](../reference/events-table.md).

## Canceling Events

Some events can be canceled:

```yaml
on: inventory_click
actions:
  - cancel_event: true
```

Use `cancel_event: false` to explicitly let a normally-blocked event through. Cancellation is supported for chat, commands, block breaking and placement, entity damage, item pickup, and inventory clicks. Handlers run in priority order and later handlers see the current cancellation state.

## Error Handling

If an action fails (an HTTP timeout, an unknown item ID, a missing inventory), Pipeline logs a warning and **continues with the next action**. The only action that stops the remaining list is `require_permission` when the check fails.

## Custom Events

Any namespaced event name can be handled like a built-in event. Fire it from an action and attach pack-defined context:

```yaml
- fire_event: "my_pack:reward"
  context:
    amount: 25
    reason: "daily_bonus"
```

A file with `on: "my_pack:reward"` receives `amount`, `reason`, and the caller's existing context. Custom events can also be invoked from functions with `pipeline:event call <name>`.
