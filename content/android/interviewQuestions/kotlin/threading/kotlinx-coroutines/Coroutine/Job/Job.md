A cancellable background job with its own lifecycle

Jobs can be arranged into parent-child hierarchies, where when parent is cancelled, it leads to immediate cancellation all of its children recursively. Failure of a child with exception other than [`CancellationException`](https://kotlinlang.org/api/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-cancellation-exception/) immediately cancels its parent and, consequently, all its other children. This behavior can be customized using [[SupervisorJob]].

```kotlin
                                          wait children
    +-----+ start  +--------+ complete   +-------------+  finish  +-----------+
    | New | -----> | Active | ---------> | Completing  | -------> | Completed |
    +-----+        +--------+            +-------------+          +-----------+
                     |  cancel / fail       |
                     |     +----------------+
                     |     |
                     V     V
                 +------------+                           finish  +-----------+
                 | Cancelling | --------------------------------> | Cancelled |
                 +------------+                                   +-----------+
```

| State                            | isActive | isCompleted | isCancelled |
| -------------------------------- | :------: | :---------: | :---------: |
| _New_ (optional initial state)   | `false`  |   `false`   |   `false`   |
| _Active_ (default initial state) |  `true`  |   `false`   |   `false`   |
| _Completing_ (transient state)   |  `true`  |   `false`   |   `false`   |
| _Cancelling_ (transient state)   | `false`  |   `false`   |   `true`    |
| _Cancelled_ (final state)        | `false`  |   `true`    |   `true`    |
| _Completed_ (final state)        | `false`  |   `true`    |   `false`   |
