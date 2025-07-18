Generics - parameterized types. The idea is to allow different data types to be a parameter to methods, classes and interfaces. It allows user to be more flexible about the data types it can operate on.

```kotlin
class Box<T>(type: T) {
    var value = type
}
```

Then, in order to create an instance of the `Box<T>`, we just have to pass the desired class as the generic :

```kotlin
val box: Box<Int> = Box<Int>(1)
```

But Kotlin is smart enough to infer the type :

```kotlin
val box = Box(1)
```

1 has type `Int`, so the compiler figures out that it is `Box<Int>`

You can learn more about generics concepts here:
[[Variance]]
[[Constraints]]
[[Type Erasure]]
[[Reified]]