`CoroutineScope` is a general figure in [[Structured Concurrency]] as it has the following:
- `CoroutineScope` **is responsible for child** [[Coroutine]]s **launched inside it, whose lifetime is attached to the lifetime of scope**. Imagine that `CoroutineScope` itself is bound to some kind of screen in an application. Network request is sent so `Coroutine` is created. User decided to leave the screen, screen is destroyed so `CoroutineScope` is, therefore all the child `Coroutines` are destroyed.
- `CoroutineScope` **automatically waits for completion of all the child** `Coroutines` **before exiting**. If `CoroutineScope` corresponds to a `Coroutine`, the parent `Coroutine` does not complete until all the child coroutines launched in this `CoroutineScope` have completed their execution.

Not only is `CoroutineScope` is responsible for utilizing **structured concurrency**, but also allows to invoke other [[suspend]] functions.

All the `Coroutine builders`([[launch]], [[async]]) are extensions for the `CoroutineScope` and provide the scope they were launched from.
## Scope Builder
In addition to multiple ways of obtaining `CoroutineScope`, such as:
- [[runBlocking]]
- [[GlobalScope]]
- `lifecycleOwner.lifecycleScope`
- `viewLifecycleOwner.lifecycleScope` and etc.
We can create `CoroutineScope` by ourselves using `coroutineScope {}` or `supervisorScope {}` just like that:

```kotlin
suspend fun doWorld() = coroutineScope {
    launch {
        delay(1000L)
        println("World!")
    }
    println("Hello")
}
```

## GlobalScope
Scope that has lifecycle that is bound to the whole application itself. On the JVM, it is the main process and on the Android, it is the [[Application]] scope. It breaks the rules of [[Structured Concurrency]] but it was designed in earlier versions of kotlinx-coroutines where no one had any idea about [[Structured Concurrency]].