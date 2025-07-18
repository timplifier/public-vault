Makes function `suspendable`. `suspend` function differs from ordinary function in the following ways:
- `suspend` function requires `CoroutineContext` to be called, e.g.  [[CoroutineScope]].
- `suspend` function allows you to directly invoke either `suspend` functions or ordinary functions, while ordinary function can't directly call `suspend` function.
- `suspend` function can be suspended and is non-blocking.

`suspend` doesn't come from any library or API, it is in Kotlin by default, though has very little usage without `kotlinx-coroutines`.

Read more about [[Coroutine]] to get familiar with it.