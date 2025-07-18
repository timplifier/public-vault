`volatile` keyword comes from Java and is used for variables. It helps to ensure that it’s value will be read only from main memory, not the thread’s one. This is important for ensuring that the visibility of changes made by one thread are visible to other threads. It might not be clear at the first sight, but here is some detailed explanation

## Visibility + No caching.

`volatile` guarantees that changes made by one thread to a shared variable are visible to other threads. Variables that are not annotated with `volatile` are stored in thread’s local cache and changes might not be immediately reflected in the main memory. `volatile` ensures that every read and write operation on the variable is performed on the main memory