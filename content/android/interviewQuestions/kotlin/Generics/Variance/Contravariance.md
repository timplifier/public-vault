Contravariance is the opposite of [[Covariance]]. It also allows you to preserve subtypes relationship, but backwards:

```kotlin
class Box<in T>(type: T)

val numberBox = Box<Number>(1)
val intBox: Box<Int> = numberBox // Works fine because of contravariance
```

You can think of contravariance like this: ”If **`A`** is a subtype of **`B`**, then `List<B>` can be a subtype of **`List<A>`**”

As we talked above about covariance and how it doesn’t work backwards, Contravariance does work backwards, but not as Covariance:

```kotlin
class Box<in T>(type: T)

val intBox = Box(1)
val numberBox: Box<Number> = intBox // Error: type mismatch
```

As [[Covariance]], Contravariance also comes with some limitations. Like with Covariance, our constructor variable `type` value can’t be used for variables. Also, we can’t use `T` as the return type in our functions, but now we can use it as a function argument