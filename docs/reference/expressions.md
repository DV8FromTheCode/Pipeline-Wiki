# Expressions Reference

Template expressions use `{{...}}` syntax and work in all `.pipe` files.

## Variables

| Variable | Description |
|---|---|
| `{{player}}` | Current player object. |
| `{{player.name}}` | Player username. |
| `{{player.uuid}}` | Player UUID. |
| `{{player.display_name}}` | Player display name. |
| `{{player.xp_level}}` | Player XP level. |
| `{{player.health}}` | Player health. |
| `{{player.op}}` | Whether the player is a server operator. |
| `{{player.online}}` | Is the player online? |
| `{{player.has_permission('...')}}` | Permission check. |
| `{{caller}}` | Command sender (player or console). |
| `{{now}}` | Current timestamp. |
| `{{pack}}` | Name of the pack that owns the running file. |

Command arguments and `storage.get` variables are also available by their name.

## Property Access

Player objects expose `name`, `display_name`, `uuid`, `xp_level`, `health`, `op`, `online`, and `has_permission('...')`. Map values (like `block` or `item.custom_data`) are accessed with dots. Unknown properties evaluate to `null`.

## Operators

| Category | Operators |
|---|---|
| Arithmetic | `+`, `-`, `*`, `/`, `%` |
| Comparison | `==`, `!=`, `<`, `>`, `<=`, `>=` |
| Logic | `&&`, `\|\|`, `!` |
| String concat | `~` |
| Ternary | `{{condition ? true_val : false_val}}` |

## Examples

```
{{player.xp_level + 1}}
{{player.op && player.health > 10}}
{{'Hello ' ~ player.display_name ~ '!'}}
{{player.health > 10 ? 'healthy' : 'hurt'}}
# balance loaded earlier with storage.get ... variable: balance
{{balance + 100}}
```

## Method Calls

```
{{player.has_permission('my_namespace.admin')}}
```

## Planned

World and server objects (`{{world.time}}`, `{{server.tps}}`) are planned but not available yet.
