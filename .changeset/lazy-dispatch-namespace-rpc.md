---
"miniflare": patch
---

Avoid opening an unused RPC WebSocket for remote dispatch namespace `get()` calls

`env.NAMESPACE.get(name)` on a remote dispatch namespace binding opened a Cap'n Web WebSocket session on every call, even when the dispatched Worker was only used through `fetch()`, which goes over plain HTTP. The unused session was dropped when the request ended, logging `Network connection lost.` locally and recording a `scriptThrewException` invocation on the remote proxy. The session is now only opened once an RPC method or property is actually used.
