Let’s imagine that you want to write a generic function that makes a `GET` network request and return the specified type at the end:

```kotlin
internal suspend fun <T> get(
    httpClient: HttpClient,
    url: URLBuilder.() -> Unit
) =
    httpClient.get {
        url {
            url()
        }
    }.body<T>() // Error: Cannot use 'T' as reified type parameter. Use a class instead.
```

The problem is that our `T` is not considered a class because type information is only available at runtime. It would be good if we could pass the class instead, isn’t it ?

Fortunately, in `Kotlin`, we can do that by using `inline` function modifier and putting `reified` keyword before the `T`:

```kotlin
internal suspend inline fun <reified T> get(
    httpClient: HttpClient,
    crossinline url: URLBuilder.() -> Unit
) =
    httpClient.get {
        url {
            url()
        }
    }.body<T>() // Works fine because of the reified.
```

Not only can we use our `T` in `body<T>()`, but also we can get declared methods for example
```kotlin
inline fun<reified T> getDeclaredMethods() = T::class.java.declaredMethods
```