# Your First Pack

Let's make the smallest possible Pipeline pack: one that says hello when a player joins.

## 1. Create the Pack Folder

Inside `run/world/datapacks/`, make this structure:

```
hello_pack/
├── pack.mcmeta
├── pipeline.json
└── data/
    └── hello/
        └── events/
            └── on_player_join.pipe
```

## 2. Add `pack.mcmeta`

```json
{
  "pack": {
    "min_format": [101, 0],
    "max_format": [101, 2],
    "description": "My first Pipeline pack"
  }
}
```

## 3. Add `pipeline.json`

```json
{
  "pipeline_format": 1,
  "name": "hello_pack",
  "version": "1.0.0",
  "description": "Greets joining players",
  "author": "YourName",
  "dependencies": {
    "pipeline_min_version": "0.1.0",
    "minecraft_min_version": "26.1"
  },
  "capabilities": ["events"]
}
```

## 4. Add the Event Handler

Create `data/hello/events/on_player_join.pipe`:

```yaml
on: player_join
priority: 10

actions:
  - tell: "@a"
    message: "&e{{player.display_name}} has joined the server!"
```

## 5. Reload

In console or in-game as an operator:

```
/pipeline reload
```

Join the server and everyone should see a yellow join message.

## What's Next?

- [Commands](../pack-format/commands.md)
- [Storage](../pack-format/storage.md)
- [Inventories](../pack-format/inventories.md)
