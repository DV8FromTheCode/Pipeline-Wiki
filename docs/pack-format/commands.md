# Commands

Make custom slash commands with arguments, permissions, and actions.

## Example

```yaml
# data/my_pack/commands/my_command.pipe
name: "my_command"
usage: "/my_command <player> [action]"
permission: "my_namespace.my_command"
aliases: ["mc"]

arguments:
  - name: player
    type: player_selector
    required: true
  - name: action
    type: string
    required: false
    default: "info"

execute:
  - if: "{{action == 'info'}}"
    then:
      - tell: "{{caller}}"
        message: "Info for {{player.display_name}}"
      - tell: "{{caller}}"
        message: "  Online: {{player.online}} | XP: {{player.xp_level}}"
  - if: "{{action == 'reset'}}"
    then:
      - require_permission: "my_namespace.my_command.reset"
      - command: "xp set {{player.name}} 0 levels"
      - tell: "{{caller}}"
        message: "&cReset XP for {{player.display_name}}"
```

## Fields

| Field | Description |
|---|---|
| `name` | Command name without the leading `/`. |
| `usage` | Help text shown to players. |
| `permission` | Permission needed to run the command. |
| `aliases` | Alternative names for the command. |
| `arguments` | List of command arguments. |
| `execute` | Actions to run when the command is used. |

## Arguments

```yaml
arguments:
  - name: <name>
    type: <type>
    required: true|false
    default: <value>
```

## Argument Types

Working now:

- `string`
- `integer`
- `decimal`
- `boolean`
- `player_selector`

!!! note
    `player_selector` currently accepts a single player name (one word) and resolves to the online player with that name, or `null` if they are offline. It does not accept `@a`/`@p` selectors.

    A `string` argument in the last position is greedy — it captures the rest of the line. In any other position it is a single word or a quoted string.

    `require_permission` inside `execute` stops the remaining actions when the check fails. It does not send a message.

Planned later:

- `location`
- `block_state`
- `item_stack`
- `enchantment`
- `potion_effect`
- `time_duration`
- `color`

## Built-in `/pipeline` Commands

| Command | Description |
|---|---|
| `/pipeline reload` | Reload events, schedules, storage, permissions, inventories, and registry. |
| `/pipeline permissions` | Operators only. Opens the approval menu. |
| `/pipeline profile` | Operators only. |
