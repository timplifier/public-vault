Direct example of inheritance is children. We inherit appearance, traits and some common things from our parents. I may look after my dad, mom. You may look after your grandmother or some relative. It constructs hierarchy. Like family tree you might have seen in some movies or you might even have it. If you don’t want to dive into details, it is pretty much it.

Let’s look at the example here:

```kotlin
class Aden(private val age: Int, private val weight: Double, private val height: Double) {

    fun introduce() {
        println("My name is Aden. I'm $age years old. I weigh $weight kg. I'm $height cm. tall.")
    }

    fun eat() {
        println("I'm eating... Om nom nom")
    }

    fun work() {
        println("I have started working...")
    }

    fun playFootball() {
        println("I like playing football. Let's go!")
    }
}

class Ethan(private val age: Int, private val weight: Double, private val height: Double) {

    fun introduce() {
        println("My name is Ethan. I'm $age years old. I weigh $weight kg. I'm $height cm. tall.")
    }

    fun eat() {
        println("I'm eating... Om nom nom")
    }

    fun work() {
        println("I have started working...")
    }

    fun playViolin() {
        println("*Playing some melody*. Wait, why the sound is so quiet?")
    }
}
```

We can spot that they are pretty much similar. They have the same constructor’s arguments, functions. Two things that are different is the name in `introduce()` function and additional methods, such as `playFootball()`, `playViolin()`.

However, we can kind of optimize it and shorten the code by defining a `Human` class that has everything that two class share:

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

So we defined a new `Human` class, that is abstract and share methods of `Aden` and `Ethan`. Also, `introduce()` , `eat()` and `work()` are annotated with `open`, which means that they can be overridden and provide different implementation for individual class. `Aden` and `Ethan` can also access their `name`, `age`, `weight` and `height`. More about method overriding here [Runtime Polymorphism](https://www.notion.so/Runtime-Polymorphism-bb8282d841754657a3729d2b4cb109a5?pvs=21)

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
        println("My name is $name. I'm $age years old. I weigh $weight kg. I'm $height m. tall.")
    }

    open fun eat() {
        println("I'm eating... Om nom nom")
    }

    open fun work() {
        println("I have started working...")
    }
}
```

Inheritance can be used to organize complex hierarchies of classes. In this case, we have a `Hierarchical Inheritance`, where our `Ethan` and `Aden` inherit from `Human`.

You can learn more about Inheritance here:
[[Single-level inheritance]]
[[Multi-level inheritance]]
[[Hybrid inheritance]]
[[Hierarchical inheritance]]
[[Multiple inheritance]]