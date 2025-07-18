It is a traditional locking mechanism provided by **Java**. It can be used in Kotlin as well.

```kotlin
val lock = Any()

synchronized(lock) {
    // critical section of code
}
```

When a thread enters `synchronized` block or method, it acquires lock, and other threads that are trying to access shared resource are blocked until the lock is released.