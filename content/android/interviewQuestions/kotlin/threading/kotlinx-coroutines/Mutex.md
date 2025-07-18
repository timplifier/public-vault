The **Mutex** ( Mutual Exclusion ) is a base type of lock that allows only one thread to access a shared resource at a time.

```kotlin
import kotlinx.coroutines.sync.Mutex

val mutex = Mutex()

suspend fun accessSharedResource() {
    mutex.lock()
    try {
        // Access the shared resource safely
    } finally {
        mutex.unlock()
    }
}
```

It provides two essential functions: `lock()` and `unlock()`. When a coroutine calls `lock()`, it acquires the lock and other coroutines that try to access the shared resource will be blocked until `unlock()` is called. This ensures thread-safe access to the shared resource. Unlike traditional locks, it is designed for coroutines. When a coroutine tries to acquire a **Mutex** that’s already held by another coroutine, it is suspended and not blocked until the lock is released.