### Microtasks and Macrotasks

| Task Type      | Examples                                          | When Executed                                  |
| -------------- | ------------------------------------------------- | ---------------------------------------------- |
| **Microtasks** | `Promise.then()`, `queueMicrotask()`              | After current operation, **before next phase** |
| **Macrotasks** | `setTimeout()`, `setInterval()`, `setImmediate()` | In **specific phases** (see below)             |

### Task Queues Mapping

| Function/Task        | Queue Type | Event Loop Phase                   |
| -------------------- | ---------- | ---------------------------------- |
| `setTimeout`         | Macrotask  | timers                             |
| `setInterval`        | Macrotask  | timers                             |
| `setImmediate`       | Macrotask  | check                              |
| `fs.readFile()`      | Macrotask  | poll → callback → check            |
| `Promise.then()`     | Microtask  | end of current tick                |
| `queueMicrotask()`   | Microtask  | end of current tick                |
| `process.nextTick()` | Microtask* | special queue before any microtask |
### Visualized Simplified Flow
```yml
┌──────────────┐
│  Call Stack  │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│ Microtasks   │ ← Promise.then, process.nextTick
└──────┬───────┘
       ▼
┌──────────────┐
│ Timers Phase │ ← setTimeout, setInterval
└──────┬───────┘
       ▼
┌──────────────┐
│ Poll Phase   │ ← I/O callbacks
└──────┬───────┘
       ▼
┌──────────────┐
│ Check Phase  │ ← setImmediate
└──────┬───────┘
       ▼
┌──────────────-┐
│Close Callbacks│ ← socket.on('close')
└──────────────-┘
```