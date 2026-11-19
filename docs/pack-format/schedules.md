# Schedules

Schedules run tasks on a timer. Put them in `data/<namespace>/schedules/`.

Each schedule file has these top-level keys:

| Key | Description |
|---|---|
| `name` | Optional. Defaults to the file name. |
| `type` | `tick`, `interval`, or `daily`. Defaults to `tick`. |
| `tick` | Integer. Meaning depends on `type` (see below). |
| `time` | `"HH:mm"` (24h). Only for `daily`. |
| `actions` | The actions to run. |

## Tick Schedule

Fires **once** when the server tick counter reaches `tick`, then the schedule is removed until the next reload. Use it for "N ticks after startup" tasks.

```yaml
# data/hello/schedules/tick_welcome.pipe
name: "tick_welcome"
type: "tick"
tick: 200

actions:
  - log: "Server has been running for 200 ticks — hello from a one-shot tick schedule!"
  - tell:
      target: "@a"
      message: "&7[Pipeline] The server tick schedule fired once after startup."
```

## Interval Schedule

Fires every `tick` ticks (20 ticks = 1 second). Only ticks are supported; there are no seconds/minutes units.

```yaml
# data/hello/schedules/interval_announce.pipe
name: "interval_announce"
type: "interval"
tick: 6000

actions:
  - log: "Interval announcement fired (every 6000 ticks / 5 minutes)"
  - tell:
      target: "@a"
      message: "&7[Pipeline] This message repeats every 6000 ticks via an interval schedule."
```

## Daily Schedule

Fires once per day, on the first tick after the server's local time passes `time` ("HH:mm", 24h). It uses the server machine's local timezone.

```yaml
# data/hello/schedules/daily_reset.pipe
name: "daily_reset"
type: "daily"
time: "02:00"

actions:
  - log: "Daily reset triggered (Phase 2)"
  - command: "say Daily reset complete"
```

## Schedule Types

| Type | Description |
|---|---|
| `tick` | One-shot. Fires once when the server tick counter reaches `tick`. |
| `interval` | Every `tick` server ticks (20 ticks = 1 second). |
| `daily` | Once per day, after local time passes `time` ("HH:mm"). |

!!! note
    Schedules run with `pack` and `now` in the context, and — if anyone is online — the first online player as `player`/`caller`. That means `{{player.name}}` in a schedule refers to an arbitrary online player.

## Planned

Cron-style schedules are planned but not supported yet.
