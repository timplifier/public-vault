Semaphore is a lock type that allows a fixed number of threads to access a shared resource concurrently. It maintains a counter that limits the number of threads that can acquire the lock. Once the limit is hit, threads trying to acquire the lock are blocked until the lock is released. It is useful when you want to control the level of concurrency. Acquiring the lock is done using `acquire()` function and releasing via `release()`.

```kotlin
import java.util.concurrent.Semaphore

val semaphore = Semaphore(3) // Allowing 3 threads concurrently

fun accessSharedResource() {
    semaphore.acquire()
    try {
        // Access the shared resource
    } finally {
        semaphore.release()
    }
}
```