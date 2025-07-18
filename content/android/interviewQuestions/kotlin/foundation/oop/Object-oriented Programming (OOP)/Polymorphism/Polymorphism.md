The word polymorphism is derived from Greek and means "**having multiple forms**." Polymorphism is basically about function / class having multiple forms of realization:

```kotlin
class Calculator {

    fun add(a: Int, b: Int): Int {
        return a + b
    }

    fun add(a: Double, b: Double): Double {
        return a + b
    }

    fun add(a: Int, b: Int, c: Int): Int {
        return a + b + c
    }
}

fun main() {
    val calculator = Calculator()

    val firstNumber = calculator.add(5, 10)
    val secondNumber = calculator.add(5.5, 4.5)
    val thirdNumber = calculator.add(1, 2, 3)
}
```

This is the example of [Compile-time Polymorphism](https://www.notion.so/Compile-time-Polymorphism-8219d8a519ac4a5c956cf18e234f6e9c?pvs=21). and it is called **method overloading**

You can learn more about polymorphism here:

[[Compile-time Polymorphism]]
[[Runtime Polymorphism]]
