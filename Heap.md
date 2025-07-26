> Stores objects and functions in memory.

By default: the function in `Nodes.js` are objects

| Feature            | **Stack**                                 | **Heap**                               |
| ------------------ | ----------------------------------------- | -------------------------------------- |
| **Allocation**     | Static (fixed size per thread)            | Dynamic (runtime, flexible size)       |
| **Speed**          | Very fast                                 | Slower                                 |
| **Access Pattern** | LIFO (Last In First Out)                  | Random access                          |
| **Management**     | Automatic (push/pop)                      | Garbage collected (GC)                 |
| **Lifetime**       | Short (function/block scope)              | Long (survives across scopes)          |
| **Error Risk**     | Stack Overflow (too deep recursion)       | Memory Leak (unused data not released) |
| **Use Cases**      | Local primitives, references, call frames | Objects, closures, lambdas, arrays     |
Link back to [[Stack]]