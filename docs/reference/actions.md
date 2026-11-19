# Actions Reference

Actions are the individual steps inside event handlers, command executors, and schedules.

## Implemented Actions

| Action | Syntax | Description |
|---|---|---|
| Command | `command: "<vanilla command>"` | Run a vanilla/server command. |
| Tell | `tell: "<target>" message: "<msg>"` | Send a message to a target. |
| Conditional | `if: "<condition>" then: [...] else: [...]` | Branch on a condition. |
| Loop (for) | `for: "<var>" in: "<list>" do: [...]` | Loop over a list. |
| Loop (while) | `while: "<condition>" do: [...]` | Loop while a condition is true. Capped at 100k iterations. |
| Storage get | `storage.get: "<key>" variable: "<name>"` | Read a storage value into a context variable. |
| Storage set | `storage.set: "<key>" value: "<val>"` | Write a storage value. |
| Storage increment | `storage.increment: "<key>" amount: N` | Increment a numeric value. |
| Storage delete | `storage.delete: "<key>"` | Delete a storage key. |
| Permission check | `require_permission: "<perm>"` | Stop executing the remaining actions if the caller lacks the permission. |
| Permission request | `request_permission: "<perm>"` | Submit a player permission request for operator review. |
| Cancel event | `cancel_event: true\|false` | Cancel or allow the triggering event. |
| Give item | `give_item: "<registry id>" to: "<target>" amount: N` | Give a registered custom item. |
| Open inventory | `open_inventory: "<id>" to: "<target>"` | Open a custom inventory. |
| Close inventory | `close_inventory: "<target>"` | Close any open Pipeline inventory. |
| Network send | `network.send: "<packet id>" to: "<target>" fields: {...}` | Send a declared packet (logs the payload for now). |
| Log | `log: "<msg>" level: "info\|warn\|error"` | Log to the console. Default level is `info`. |
| HTTP GET | `http_get: "<url>" headers: {...} store: "<variable>"` | Fetch an HTTP resource and store its body, status, and headers in context. |
| HTTP POST | `http_post: "<url>" headers: {...} body: {...} store: "<variable>"` | POST a JSON body and store the response in context. |
| Fire event | `fire_event: "<namespace:event>" context: {...}` | Fire a custom event with additional context. |

`tell` also accepts a map form: `tell: { target: "@a", message: "..." }`.

HTTP responses use the requested variable name for the body and add `<variable>_status` and `<variable>_headers`. Requests allow only public HTTP(S) targets, do not follow redirects, time out after 10 seconds, and reject responses larger than 1 MiB.

## Planned Actions

| Action | Syntax | Status |
|---|---|---|
| Tell JSON | `tell_json: "<target>" json: {...}` | Planned |
| Run function | `run_function: "<ns:function>"` | Planned |
| Run pipe | `run_pipe: "<ns/file>"` | Planned |
| Wait | `wait: "<duration>"` | Planned |
