# Permissions

Declare permissions in `data/<namespace>/permissions/`.

## Example

```yaml
# data/my_pack/permissions/permissions.pipe
permissions:
  - id: "my_namespace.my_command"
    description: "Use /my_command"
    default: "op"

  - id: "my_namespace.my_command.reset"
    description: "Reset player data via /my_command"
    default: "false"

  - id: "my_namespace.fly"
    description: "Toggle flight mode"
    default: "true"

  - id: "my_namespace.economy.admin"
    description: "Administer economy"
    default: "op"
    children:
      - "my_namespace.economy.give"
      - "my_namespace.economy.take"
      - "my_namespace.economy.set"
```

## Fields

| Field | Description |
|---|---|
| `id` | Permission node. |
| `description` | Human-readable description. |
| `default` | Default access: `true`, `false`, or `op`. |
| `children` | Child permissions granted implicitly. |

## Default Levels

| Default | Meaning |
|---|---|
| `true` | Everyone has it. |
| `false` | Nobody has it until granted through an approved request (see below). Operators do **not** get it automatically. |
| `op` | Server operators have it; others need an approved request. |

A permission that is checked but never declared in any `permissions/` file behaves like `op`. Any unrecognised `default` value also behaves like `op`.

## Granting Permissions

There is no `/pipeline grant` command. The only way a non-operator gains a `false`/`op` permission is:

1. A pack runs `request_permission` for that player.
2. An operator approves the request in `/pipeline permissions`.

The resulting grant is permanent and stored in `pipeline/permissions.json` next to `pipeline.jar`. To revoke a grant, stop the server and remove the entry from that file. Granting a parent permission also grants every ID listed in its `children`.

## Checking Permissions

From an expression:

```yaml
- if: "{{player.has_permission('my_namespace.admin')}}"
```

From an action:

```yaml
- require_permission: "my_namespace.my_command"
```

## Approval Requests

A player-context action can submit a request for a permission:

```yaml
- request_permission: "my_namespace.fly"
```

The action needs a player in the context (`player` or `caller`); from the console or a schedule with nobody online it logs a warning and does nothing. If the same player already has a pending request for the same permission, no duplicate is created.

Requests persist in `pipeline/permission-requests.json`. Operators review them with `/pipeline permissions`. The in-game menu approves a request with left click or denies it with right click; approval immediately creates the normal persistent player grant.
