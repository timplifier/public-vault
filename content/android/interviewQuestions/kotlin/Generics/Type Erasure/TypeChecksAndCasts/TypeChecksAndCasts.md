Due to the [[Type Erasure]], there is no general way to check whether an instance of generic type was created with a certain type argument at runtime. It is possible to make a check against an instance of star-projection

```kotlin
if (something is List<*>) {
    something.forEach { println(it) } // The items are typed as `Any?`
}
```

If you already have a defined type argument, you can check non-generic part of the type.

```kotlin
fun handleStrings(list: MutableList<String>) {
    if (list is ArrayList) {
        // `list` is smart-cast to `ArrayList<String>`
    }
}
```

Again, due to [Type erasure](https://www.notion.so/Type-erasure-c03950960afc484086feb8df8a8924ba?pvs=21), you can’t use type parameters for type checks and type casts to type parameters are `unchecked`.

```kotlin
class Box<T> {
    fun someFun(value: Any) {
        if (value is T) { // Error: Cannot check for instance of erased type: T
            ...
        }
    }
} 
```

Learn more about casts:
[[SmartCast]]
[[UncheckedCast]]