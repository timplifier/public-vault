An element of the [[CoroutineContext]]. CoroutineContext inheritance means that every value from the CoroutineContext is effectively CoroutineContext itself.
```kotlin
public interface Element : kotlin.coroutines.CoroutineContext {
        public abstract val key: kotlin.coroutines.CoroutineContext.Key<*>

        public open fun <R> fold(initial: R, operation: (R, kotlin.coroutines.CoroutineContext.Element) -> R): R 
        public open operator fun <E : kotlin.coroutines.CoroutineContext.Element> get(key: kotlin.coroutines.CoroutineContext.Key<E>): E? 

        public open fun minusKey(key: kotlin.coroutines.CoroutineContext.Key<*>): kotlin.coroutines.CoroutineContext 
    }
```