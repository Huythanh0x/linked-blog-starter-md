> Follow <mark style="background: #ADCCFFA6;">`LIFO`</mark>

**New function** calls are **pushed on top**.
The top must **finish** before the next one below can continue.

*Example* is the recursive function. Every new function call would be added to stack and endless calls would lead to

### [[Stack]] and [[heap]]
- primitive value (int, boolean) → [[stack]]
- object:
	- `obj` → [[stack]] (just a pointer)
	- `name`, `"Thanh"`, `age`, `25` → stored in the [[heap]], as part of the object

```yml
Stack:
┌────────────┐
│ obj ─────┐ │
└────────────┘
             ↓
         (reference)
Heap:
┌──────────────────────────────┐
│ { name: "Thanh", age: 25 }   │
└──────────────────────────────┘
```
Link back to [[Heap]]

### Function calls and Stack:
Each time a function is called:
- A new **stack frame** is created for that function.
- It includes: parameters, local variables, return address, etc. [[Function signature]]
- When the function finishes, that stack frame is **popped**.

### Stack-overflow
> If the stack is full and a new function is called