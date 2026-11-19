# MCFunction Integration

Normal `.mcfunction` files can call Pipeline commands with a `pipeline:` prefix.

## Example

```mcfunction
# data/my_pack/function/my_func.mcfunction
execute as @a run say Hello
pipeline:storage set my_pack:player_data.{{player.uuid}}.score 10
pipeline:event call my_namespace:on_score_update
pipeline:command run greet {{player.name}} hello
```

## Commands

| Command | Description |
|---|---|
| `pipeline:storage set <pack:key> <value>` | Set a storage value. |
| `pipeline:storage get <pack:key>` | Read a storage value. |
| `pipeline:storage increment <pack:key> <amount>` | Increment a numeric value. |
| `pipeline:storage delete <pack:key>` | Delete a storage key. |
| `pipeline:event call <namespace:path>` | Trigger a custom event. |
| `pipeline:command run <command line>` | Run a Pipeline command line as the current command source. |

Storage keys must be prefixed with the pack name. The pack prefix is the `name` from that pack's `pipeline.json`.

`{{...}}` templates in these commands only resolve `player`/`caller`/`now` when the executing source is a player (e.g. `execute as @a run pipeline:storage ...`).

This lets you adopt Pipeline features gradually — existing data packs keep working and you can add Pipeline logic one line at a time.
