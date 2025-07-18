The [[CoroutineContext]] includes a [**coroutine dispatcher**](https://kotlinlang.org/api/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-coroutine-dispatcher/)that determines what threads [[Coroutine]]  is allowed to use for its execution.

All [[Coroutine]] builders like [[launch]] and [[async]] accept an optional [[CoroutineContext]] that is used to explicitly specify the dispatcher for new coroutine.

So the following example
```kotlin
runBlocking { // no context specified, will get dispatched to MainDispatcher
        launch { // context of the parent, main runBlocking coroutine
            println("main runBlocking      : I'm working in thread ${Thread.currentThread().name}")
        }
        launch(Dispatchers.Unconfined) { // not confined -- will work with main thread
            println("Unconfined            : I'm working in thread ${Thread.currentThread().name}")
        }
        launch(Dispatchers.Default) { // will get dispatched to DefaultDispatcher 
            println("Default               : I'm working in thread ${Thread.currentThread().name}")
        }
        launch(newSingleThreadContext("MyOwnThread")) { // will get its own new thread
            println("newSingleThreadContext: I'm working in thread ${Thread.currentThread().name}")
        }
    }
```
Will produce the following output
```
Unconfined            : I'm working in thread main
Default               : I'm working in thread DefaultDispatcher-worker-1
newSingleThreadContext: I'm working in thread MyOwnThread
main runBlocking      : I'm working in thread main
```

When any coroutine builder is used without parameters, it is going to inherit the [[CoroutineContext]] from the [[CoroutineScope]] it is being launched from.

You can check out all the Dispatchers here:
- [[Main]]
- [[IO]]
- [[Default]]
- [[Unconfined]]
- [[newSingleThreadContext]]