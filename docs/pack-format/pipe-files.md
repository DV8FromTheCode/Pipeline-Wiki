# .pipe Files

Pipeline-specific files use the `.pipe` extension and YAML syntax. Where you put a `.pipe` file tells Pipeline what to do with it.

## Layout

```
data/<namespace>/
  commands/     # custom slash commands
  events/       # event handlers
  schedules/    # timed/recurring tasks
  permissions/  # permission declarations
  storage/      # data schemas
  inventories/  # GUI definitions
  registry/     # custom items, blocks, packets
  network/      # custom packet schemas
```

## Writing .pipe Files

- UTF-8 encoding
- 2-space indentation
- Minecraft color codes like `&a` and `&b` work in strings
- Template expressions use `{{...}}`
- The action list key is `actions`; `execute` is accepted as an alias in every file type
- The **immediate parent folder** decides the file type. `events/on_join.pipe` is an event handler; `events/player/on_join.pipe` is ignored, and so is any file in a folder not listed above

## Template Expressions

Expressions show up inside `{{...}}` and work in every `.pipe` file.

```yaml
message: "&eWelcome, {{player.display_name}}!"
```

You can do:

- Variable access: `{{player}}`, `{{now}}`, `{{caller}}`
- Dotted access: `{{player.name}}`, `{{block.x}}`
- Arithmetic: `+`, `-`, `*`, `/`, `%`
- Comparisons: `==`, `!=`, `<`, `>`, `<=`, `>=`
- Logic: `&&`, `\|\|`, `!`
- String concatenation: `~`
- Ternary: `{{condition ? true_val : false_val}}`
- Method calls: `{{player.has_permission('my_namespace.admin')}}`

See the [Expressions Reference](../reference/expressions.md) for the full list.
