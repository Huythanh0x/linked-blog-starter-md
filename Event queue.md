>A queue of callbacks waiting to be pushed to the [[call stack]].

Also referred as [[Callback Queue]]

**Flow**
1. Asynchronous tasks (e.g., `setTimeout`, `fs.readFile`) complete in background.
2. Their callbacks are pushed into the [[Event queue]]
3. When the [[call stack]] is empty, Node.js pulls the next item from the [[event queue]] and runs it

