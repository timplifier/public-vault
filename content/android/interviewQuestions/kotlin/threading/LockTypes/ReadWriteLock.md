ReadWriteLock allows concurrent access to a shared resource for read operations, but exclusive access for write operations. Multiple threads can acquire the read lock simultaneously, but only one thread can acquire the write lock. This lock type is more suitable than other ones when read operations happen more often than the write ones.

```kotlin
import java.util.concurrent.locks.ReentrantReadWriteLock

val readWriteLock = ReentrantReadWriteLock()

fun readSharedResource() {
    readWriteLock.readLock().lock()
    try {
        // Read from the shared resource
    } finally {
        readWriteLock.readLock().unlock()
    }
}

fun writeSharedResource() {
    readWriteLock.writeLock().lock()
    try {
        // Write to the shared resource
    } finally {
        readWriteLock.writeLock().unlock()
    }
}
```