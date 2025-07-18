If you want to constrain your generic type parameter to be a subtype of some supertype, you can use generic constraints.

## Upper bounds

The most common type of generic constraints is upper bound, that allows you to accept a generic type that is a subtype to some supertype.

```kotlin
fun <T : Fish> sort(list: List<T>) {  ... }
```

The type specified after a `:` is an upper bound. So in this function we only accept subtypes of `Fish`.

How it works can be demonstrated in the following manner:

```kotlin
sort(listOf(Carp(), Sardine())) // OK. Carp & Sardine is a subtype of Fish
sort(listOf(Cat(), Dog())) // Error: Cat & Dog are not a subtype of Fish
```

The default upper bound if nothing specified is `Any?`. Only one upper bound can be specified in `<>`. In case that the same type parameter needs more than one upper bound, you can use `where` clause.

```kotlin
fun <T : Fish> sort(list: List<T>) where T : Crab, T  : Potamidae {  ... }
```

The passed type must satisfy both upper bound in `<>` and `where` clause. In the above example, `T` must be a `Fish` subtype, `Crab` subtype and `Potamidae` subtype.