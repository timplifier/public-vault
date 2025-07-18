By default, Kotlin's generic types are invariant, which means that no subtype relationship exists between instances of the same type with different type arguments. Let’s return to our example with the `Box`:

```kotlin
class Box<T>(type: T) {
    var value = type
}
val intBox = Box(1) // Works fine
val numberBox: Box<Number> = intBox // Error: Type mismatch
```

Even though `Int` is a subtype of `Number`, `Box<Int>` is not a subtype of `Box<Number>` because there is no subtype relationship in the invariance.

Not only does invariant generic means that there is no relationship preserved between instances of the same type but with different type arguments, but also that the type argument can be used for fields and both functions that take in the type as a parameter and return it as a result.
```kotlin
class Box<T>(type: T) {
    var value = type

    fun modifyType(type: T): T {
        ...
        return type
    }
}
```