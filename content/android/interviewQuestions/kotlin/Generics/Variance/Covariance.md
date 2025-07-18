Covariance allows you to preserve the subtype relationship between instances of the same type but with different type arguments

```kotlin
class Box<out T>(type: T) 

val intBox = Box(1) 
val numberBox: Box<Number> = intBox // Works fine because of covariance
```

You can think of covariance like this: ”If **`A`** is a subtype of **`B`**, then `List<A>` can be a subtype of **`List<B>`**”

On the one hand, it works fine with subtyping, but on the other hand, it doesn’t work backwards.

```kotlin
class Box<out T>(type: T) 

val numberBox = Box<Number>(1)
val intBox: Box<Int> = numberBox // Error: type mismatch
```

There is always something you have to pay with. Covariance comes with some limitations. Now, we can’t define new variable that uses our constructor variable `type` value as before. And our function `modifyType` cannot take our `T` as an argument, but we still can use it as a return type.
```kotlin
class Box<out T>(type: T) {

    fun modifyType(): T {
        ...
        return type
    }
}
```