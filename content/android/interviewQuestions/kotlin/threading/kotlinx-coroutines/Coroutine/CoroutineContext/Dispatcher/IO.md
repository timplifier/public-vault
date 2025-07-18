A [[Dispatcher]] that is confined to kind of `background thread`, specializing on input/output tasks, which he inherits its name from. The IO Dispatcher has a large thread pool, that is by default set to either 64 or the number of CPU cores available to the system. This value can be configured by modifying the value of `limitedParallelism` like that: 
```kotlin
Dispatchers.IO.limitedParallelism(100)
```
\
Additional threads in this pool are created on demand.

With this large amount of threads, it makes IO suitable for blocking operations, such as reading/writing files, performing database queries and making network requests.

In general, IO operations doesn't consume a lot of CPU resources, but rather take a long time to finish. Therefore, having access to this amount of threads can boost our application's ability to finish them faster.

IO Dispatcher doesn't block the operations that are executed on [[Main]] or other coroutines running on [[Default]] Dispatcher.