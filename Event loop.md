![[The Psychology of Money (Morgan Housel) (Z-Library).epub]]> Core mechanism in [[Node.js]] that enables **non-blocking**, asynchronous behavior using a single thread.

[[Event loop]] lets Node.js **offload work**, and then run callbacks when results are ready.
### Flow
```yml
          ┌─────────────┐
          │ Call Stack  │  <───── Executes functions (sync + async callbacks)
          └────▲────────┘
               │
               │ (callback ready)
          ┌────┴────┐
          │ Event   │  <───── Callback queue
          │ Queue   │
          └────▲────┘
               │
      ┌────────┴────────┐
      │    Event Loop   │  <──── Coordinates Call Stack & Event Queue
      └─────▲─────┬─────┘
            │     │
      ┌─────┘     └─────────┐
      │                     │
┌─────┴──────┐      ┌───────┴────────┐
│ Timer APIs │      │ libuv (I/O, FS)│
└────────────┘      └────────────────┘
```
**Components**
- [[Call stack]]
- [[Heap]] [[Stack]]
- [[Event queue]]
- [[libuv]]
- [[Task queues]]

### Event Loop phases
Each iteration is called a **tick**, and inside each tick, [[Node.js]] runs **phases** in order:

| Phase                    | Description                                                           |
| ------------------------ | --------------------------------------------------------------------- |
| **1. timers**            | Executes callbacks scheduled by `setTimeout` and `setInterval`        |
| **2. pending callbacks** | Executes some system-level callbacks (e.g., TCP errors)               |
| **3. idle / prepare**    | Used internally by Node.js (libuv)                                    |
| **4. poll**              | Retrieves I/O events (e.g., file system, sockets), waits if necessary |
| **5. check**             | Executes callbacks scheduled by `setImmediate()`                      |
| **6. close callbacks**   | Executes `close` events (e.g., `socket.on('close')`)                  |

