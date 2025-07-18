# **Type erasure** - process of removing explicit type annotations in runtime. It enforces type constraints only at compile time and their discard at runtime

## Benefits

Type Erasure provide several benefits to the language that use Generics:

1. **Backward Compatibility**. **Type erasure** makes it possible for the language to interact with Generics interruptibly with older versions of the language that don't have Generics.
2. **Runtime Efficiency**. Due to runtime **Type Erasure**, program has more efficient bytecode resulting in less memory used and less overhead. The program's type system is simplified when it is executed, thus reducing the performance costs and additional complexity due to specializing every type.
3. **Interoperability**. Languages with Type erasure, for example **Kotlin**, can use other languages's libraries which also rely on **Type erasure**, such as **Java.**

However, **Type erasure** has also some disadvantages that comes into play when using it:

## Disadvantages

1. **Loss of Type Information**. Once the type information is _**erased**_, it can't be recovered at runtime. This makes certain kinds of **reflection** or **runtime checks _impossible_**. In addition, you are forced to use unchecked casts that can possibly fail at runtime and often throws `ClassCastException`. Consequently, the performance overhead can be introduced due to use of runtime casts and runtime type checks
2. **Type uncertainty**. Types are _**erased**_ at runtime, so we are not allowed to use some programming techniques and interacting with types at runtime can be difficult. For example, you **can't create an instance of a generic type `T` within a generic method or class**.
3. **Verbose Code**. **Type erasure** often **forces you to write additional boilerplate code**, such as _**passing `Class<T>` objects as parameters to methods where you need to know the type**_.
4. **Debugging Difficulties**. Dealing with exceptions related to types that occur only in runtime can be challenging enough to consume a lot of time for a developer compared to languages that maintain type information.

You can get yourself familiar with type checks
[[TypeChecksAndCasts]]