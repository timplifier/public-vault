**ReentlackLock** is an advanced lock mechanism that offers more flexibility compared to the [synchronized](https://www.notion.so/synchronized-d96f478dd9664fb0a526e7a49cf205a0?pvs=21). It’s **reentrant**, meaning the same thread can acquire lock multiple times without causing a [[Deadlock]].
```kotlin
import java.util.concurrent.locks.ReentrantLock

fun accessSharedResource() {
    val lock = ReentrantLock()

    lock.lock()
    try {
        // critical section of code
    } finally {
        lock.unlock()
    }
}
```

Like [synchronized](https://www.notion.so/synchronized-d96f478dd9664fb0a526e7a49cf205a0?pvs=21), **ReentrantLock** is blocking, meaning that any thread that try to access the already acquired lock will the blocked.