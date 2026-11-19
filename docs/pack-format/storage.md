# Storage

Storage lets you save persistent data. Each pack gets its own isolated storage. Pipeline uses JSON files by default and can store the same partition data in SQLite.

## Example

```yaml
# data/my_pack/storage/player_data.pipe
schema:
  key: "player_data"
  type: "player_bound"

  fields:
    last_login:
      type: "timestamp"
      default: null
    login_count:
      type: "integer"
      default: 0
    balance:
      type: "decimal"
      default: 0.0
    rank:
      type: "string"
      default: "member"
    homes:
      type: "list<location>"
      default: []
    metadata:
      type: "map<string,string>"
      default: {}
```

## Schema Types

| Type | Description |
|---|---|
| `global` | Shared across all players. |
| `player_bound` | Split by player UUID. |
| `world_bound` | One entry per world. |

## Field Types

| Type | Aliases | Stored as |
|---|---|---|
| `string` | — | text |
| `integer` | `int` | whole number |
| `decimal` | `double`, `float` | floating-point number |
| `boolean` | `bool` | `true`/`false` |
| `timestamp` | — | text (ISO-8601 string, e.g. the value of `{{now}}`) |
| `location` | — | stored as-is |
| `list<T>` | — | stored as-is |
| `map<K,V>` | — | map; non-map values become `{}` |
| `json` | — | stored as-is |

Type parameters like `list<location>` are not validated. Fields that are not declared in the schema are stored with whatever type the value has.

## Keys

A storage key is `<schema>.<field>` for `global` schemas and `<schema>.<partition>.<field>` for `player_bound`/`world_bound` schemas. The partition is whatever you put in the second segment, usually `{{player.uuid}}`. Field names can contain further dots (`metadata.temp_flag`).

Keys default to the pack running the action. Prefix a key with another pack's name and a colon to read or write that pack's storage:

```yaml
- storage.get: "economy_pack:player_data.{{player.uuid}}.balance"
  variable: balance
```

The pack name is the `name` from that pack's `pipeline.json`.

## Actions

```yaml
# Read into a context variable
- storage.get: "player_data.{{player.uuid}}.balance"
  variable: balance
- tell: "{{player}}"
  message: "Balance: {{balance}}"

# Write
- storage.set: "player_data.{{player.uuid}}.balance"
  value: 100

# Increment
- storage.increment: "player_data.{{player.uuid}}.login_count"
  amount: 1

# Delete
- storage.delete: "player_data.{{player.uuid}}.metadata.temp_flag"
```

## Storage Backend

The server owner selects one backend for all packs in `run/pipeline.json`. Existing servers need no configuration and continue using the `storage/` folder next to `pipeline.jar`:

```json
{
  "storage": {
    "backend": "sqlite",
    "path": "storage/pipeline.db"
  }
}
```

Supported backends are `json` and `sqlite`. Paths are resolved from the server working directory. Switching backends does not migrate existing data; copy or migrate it before changing the setting. Remote storage remains deferred until its protocol is specified.
