**Compile-time polymorphism** is usually associated with **Method overloading**, which is the direct implementation of **compile-time polymorphism**. However, it has more types than you’d expected.

## Method overloading

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

In this example, we can see that `Calculator` has multiple `add` methods, but these methods have different arguments, even though they also have different return types, it is not important to overload method. **Method overloading** occurs via different function arguments. So if we had this calculator:

```kotlin
class Calculator {

    fun add(a: Int, b: Int): Int {
        return a + b
    }

    fun add(a: Double, b: Double): Double {
        return a + b
    }

    fun add(a: Double, b: Double): Boolean {
        ...
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
    val fourthNumber = calculator.add(6.5, 3.5) // Error: Conflicting overloads.
}
```

When we try to create our `fourthNumber`, we will get a compiler error with conflicting overloads, because we have two `add`methods that accept two arguments of type `Double`, tho they have different return types, it doesn’t allow us to have different methods and overload.

## Operator overloading

The second form of compile-time polymorphism is **Operator Overloading.** We are able to overload predefined operators with our own implementation. In this case, we overrode `invoke`, `plus` and `minus` operators. Previously, they weren’t available to use, but now, we can use them easily and however we want:

```kotlin
fun main() {
    val p1 = Point(3, 4)
    p1()
    val p2 = Point(1, 2)
    p2()

    val sum = p1 + p2
    val difference = p1 - p2

    println("Sum: $sum")
    println("Difference: $difference")
}

data class Point(val x: Int, val y: Int) {
    
    operator fun invoke() {
        println("I'm point with x: $x and y: $y")
    }

    operator fun plus(other: Point): Point {
        return Point(x + other.x, y + other.y)
    }

    operator fun minus(other: Point): Point {
        return Point(x - other.x, y - other.y)
    }
}
```

Some of you might have caught themselves on a thought that it is about overriding rather than overloading. But it is a form of **compile-time** polymorphism and this behavior is determined at **compile-time** rather than runtime. Also, it doesn’t require any inheritance like in [Runtime Polymorphism](https://www.notion.so/Runtime-Polymorphism-bb8282d841754657a3729d2b4cb109a5?pvs=21).

## Generics

The third and final form of **compile-time polymorphism** is generics. If you are not familiar with this concept, you can check out [Generics](https://www.notion.so/Generics-1cb2e6fd5ff64c1c813f4bcf901b3009?pvs=21) article. Generics can work with variety of different types. So generic function is considered as **compile-time polymorphism.** For example:

```kotlin
fun <T> printList(list: List<T>) {
    list.forEach { println(it) }
}
```

We can see that our function accepts any class as a type parameter. So we can use it like that:

```kotlin
fun <T> printList(list: List<T>) {
    list.forEach { println(it) }
}

fun main() {
    val numList = listOf(1, 2, 3, 4)
    printList(numList)
    val stringList = listOf("abc", "bca", "acb")
    printList(stringList)
}
```

Generics by themselves are the strongest example of polymorphism, because if you want to use generics, you want to operate on multiple forms you don’t even expect. The important point is that they don’t modify objects behavior in runtime. They serve as documentation for the compiler in **compile-time**, and are erased at runtime. I strongly recommend looking at [Generics](https://www.notion.so/Generics-1cb2e6fd5ff64c1c813f4bcf901b3009?pvs=21) article