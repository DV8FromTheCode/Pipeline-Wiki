# pipeline.json

Every Pipeline pack needs a `pipeline.json` at the root. It tells Pipeline what your pack is, what it needs, and what features it uses.

## Example

```json
{
  "pipeline_format": 1,
  "name": "my_pack",
  "version": "1.0.0",
  "description": "My awesome Pipeline pack",
  "author": "AuthorName",
  "dependencies": {
    "pipeline_min_version": "0.3.0",
    "minecraft_min_version": "26.1",
    "requires": [
      { "id": "other_pack", "min_version": "2.0.0" }
    ]
  },
  "permissions": [
    "my_namespace.admin",
    "my_namespace.economy"
  ],
  "capabilities": [
    "commands",
    "events",
    "schedules",
    "storage",
    "registry",
    "network",
    "inventories"
  ]
}
```

## Fields

| Field | Read by loader | Description |
|---|---|---|
| `pipeline_format` | No | Format version. Use `1`. Reserved for future compatibility checks. |
| `name` | Yes | Pack identifier. Used as the storage prefix and in `requires`. Defaults to the folder name if omitted, but always set it explicitly. |
| `version` | Yes | SemVer version string. Defaults to `0.0.0`. Other packs' `requires` compare against this. |
| `description` | No | Human-readable description. |
| `author` | No | Author name. |
| `dependencies` | Yes | Minimum versions and required packs. A pack that fails any check is skipped with a warning in the log. |
| `permissions` | No | Permissions this pack may grant. Informational for now; not read by the loader. |
| `capabilities` | No | Feature areas the pack uses. Informational for now; not read by the loader. |

Only the file's presence makes a folder a Pipeline pack; every field has a default. Still, treat `name` and `version` as mandatory in practice.

## Dependencies

| Field | Description |
|---|---|
| `pipeline_min_version` | Minimum Pipeline version required. |
| `minecraft_min_version` | Minimum Minecraft version required. |
| `requires` | Other packs this pack depends on, with `id` and `min_version`. |

## Capabilities

Informational for now; not read by the loader.

List the Pipeline subsystems your pack touches:

- `commands`
- `events`
- `schedules`
- `storage`
- `permissions`
- `registry`
- `inventories`
- `network`
- `http`
