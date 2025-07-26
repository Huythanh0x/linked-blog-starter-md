> **Asynchronous** operations are non-blocking. Instead of waiting for a task (like a network or disk call) to finish, the program **continues executing** and handles the result later.

### But why?
- Prevent [[main thread]] from being stalled
- Effectively handles many tasks at once

### Core components on NodeJs
- Single threaded [[Event loop]]
- Use [[callbacks]], [[Promises]] or [[async-await]]
- Powered by [[libuv]] for [[thread pool]] and [[I/O offloading]]