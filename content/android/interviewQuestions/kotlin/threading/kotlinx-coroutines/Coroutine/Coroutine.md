Instance of a `suspendable computation`. It is similar to the thread in the sense that it takes a block of code and runs it concurrently with the rest of the code. However, `Coroutine` itself is not bound to a particular thread and instead can start in one thread, suspend its execution and then resume on another thread.

What do you think the output of the following code block would be ? 
```kotlin
fun main() = runBlocking {
    launch {
        delay(1000L)
        println("World!")
    }
    println("Hello")
}
```

If you run it, you will see that it will print:

```
Hello
World!
```
And why is that ? Let's find out:

1. **The whole function is launched in [[CoroutineScope]]**. We use [[runBlocking]], it means that it is our main `Coroutine` so it provides the [[CoroutineScope]].
2. `"World!"` **is launched in another** `Coroutine`. Printing `"World!"` is done using another `Coroutine` via [[launch]] so it runs independently.
3. **Printing** `"World!"` **is delayed**.  Using [[delay]], we can suspend the execution of `Coroutine` and resume it after some time, in our case, one second, so it allows to print `"Hello"` first.
If you remove [[runBlocking]], **this code will not compile because** `launch` **is an extension for** `CoroutineScope`.

## Structured Concurrency.

`Coroutines` follow a principle of **structured concurrency**, which essentially means that new coroutines can only be launched in a specific `CoroutineScope` that delimits the lifetime of the coroutine, so when `CoroutineScope` is destroyed, all the coroutines launched in this `CoroutineScope` are also cancelled so there is no memory leak. 

Comparing `kotlinx-coroutines` to `RxJava`, the second doesn't follow the principle of **structured concurrency**, because API doesn't force you to collect and clear launched [[Disposable]]s, so occurrence of memory leak is increased. 

**Structured concurrency** ensures that launched coroutines are not lost and leaked. `CoroutineScope` cannot be complete until all the child coroutines complete. As we can see in our example, block is not completed until `"World!"` is printed and waits until it is completed and only then exits.

`kotlinx-coroutines` takes it to the whole new level by prohibiting start of new `Coroutines` outside of the `CoroutineScope`.