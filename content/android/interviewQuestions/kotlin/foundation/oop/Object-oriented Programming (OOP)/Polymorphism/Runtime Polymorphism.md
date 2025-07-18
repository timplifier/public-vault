**Runtime polymorphism** is usually associated with **Method overriding**, which is the direct implementation of **runtime polymorphism.** However, it holds more types than you expect.

## Method overriding

```kotlin
class Aden : Human("Aden", 18, 55.0, 170.0) {

    fun playFootball() {
        println("I like playing football. Let's go!")
    }
}

class Ethan :
    Human("Ethan", 21, 70.0, 185.0) {

    fun playViolin() {
        println("*Playing some melody*. Wait, why the sound is so quiet?")
    }
}

abstract class Human(
    protected val name: String,
    protected val age: Int,
    protected val weight: Double,
    protected val height: Double
) {

    fun introduce() {
        println("My name is $name. I'm $age years old. I weigh $weight kg. I'm $height cm. tall.")
    }

    fun eat() {
        println("I'm eating... Om nom nom")
    }

    fun work() {
        println("I have started working...")
    }
}
```

Let’s assume that we want to have a custom implementation of `introduce()` method in `Ethan` and `Aden` respectively. It is achievable via `open`.

```kotlin
class Aden : Human("Aden", 18, 55.0, 170.0) {

    fun playFootball() {
        println("I like playing football. Let's go!")
    }
}

class Ethan :
    Human("Ethan", 21, 70.0, 185.0) {

    fun playViolin() {
        println("*Playing some melody*. Wait, why the sound is so quiet?")
    }

    override fun introduce() {
        println("Hello motherfucker, I'm Ethan. I'm already 21 and I can fuck you any moment. I weigh 70.0 kg and I'm fucking 185.0 cm tall dude.")
    }
}

abstract class Human(
    protected val name: String,
    protected val age: Int,
    protected val weight: Double,
    protected val height: Double
) {

    open fun introduce() {
        println("My name is $name. I'm $age years old. I weigh $weight kg. I'm $height cm. tall.")
    }

    open fun eat() {
        println("I'm eating... Om nom nom")
    }

    open fun work() {
        println("I have started working...")
    }
}
```

Now we are able to override the `introduce()` method and write our own implementation. The same thing is applied with `interface`, `open class` as well.

## Type inference

Another kind of type of runtime polymorphism is **Type inference.** It can be seen in functions and classes that require supertype, rather than child class

```kotlin
abstract class Human(
    protected val name: String,
    protected val age: Int,
    protected val weight: Double,
    protected val height: Double
) {

    open fun introduce() {
        println("My name is $name. I'm $age years old. I weigh $weight kg. I'm $height cm. tall.")
    }

    open fun eat() {
        println("I'm eating... Om nom nom")
    }

    open fun work() {
        println("I have started working...")
    }
}

abstract class Student(
    name: String,
    age: Int,
    weight: Double,
    height: Double,
    var gpa: Float,
    var className: String
) : Human(name, age, weight, height) {

    override fun introduce() {
        super.introduce()
        println("My current gpa is $gpa and I study at $className")
    }

    fun study() {
        if (Random.nextBoolean())
            gpa += 0.1f
    }
}

class Derek : Student("Derek", 17, 55.0, 168.0, 2.1f, "1A")

class School {

    fun introduceStudent(student: Student) {
        student.introduce()
    }
}

fun main() {
    val derek = Derek()
    val school = School()
    school.introduceStudent(derek)
}
```

So in this example, we constructed more complex hierarchy and introduced `Student` as a new abstract class that inherits from `Human`. In our `School`, in `introduceStudent()` method we accept `Student` as a parameter, which is a supertype rather than a child, like `Derek`. So we don’t care whether our student would inherit from other classes, it should be a child of `Student`.