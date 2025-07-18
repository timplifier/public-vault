Let’s look at this piece of code:

```kotlin
class CreditCardPayment {
    fun processAmount(amount: Double) {
        println("Processing $amount using Credit Card")
        ...
    }
}

class PayPalPayment {
    fun transactAmount(amount: Double) {
        println("Transacting $amount using PayPal")
        ...
    }
}

class Amazon {
    val creditCardPayment = CreditCardPayment()
    val payPalPayment = PayPalPayment()
}

fun main() {
    val amount = Random.nextDouble(from = 100.0, until = 150.0)
    val isCard = Random.nextBoolean()

    val amazon = Amazon()
    if (isCard) {
        amazon.creditCardPayment.processAmount(amount)
    } else {
        amazon.payPalPayment.transactAmount(amount)
    }
}
```

We can see that we have to hold both payment methods in our `Amazon` class, which is not good. They are hardcoded. Now let’s imagine that we will have more than 10 payment methods. Are we going to have 10 variables defined as payment methods ? Of course not

Let’s look at an upgraded example:

```kotlin
interface PaymentMethod {
    fun processPayment(amount: Double)
}

class CreditCardPayment : PaymentMethod {
    override fun processPayment(amount: Double) {
        println("Processing $amount using Credit Card")
        ...
    }
}

class PayPalPayment : PaymentMethod {
    override fun processPayment(amount: Double) {
        println("Transacting $amount using PayPal")
        ...
    }
}

class Amazon(private val paymentMethod: PaymentMethod) {
    fun processPayment(amount: Double) = paymentMethod.processPayment(amount)
}

fun main(paymentMethod: PaymentMethod) {
    val amount = Random.nextDouble(from = 100.0, until = 150.0)

    val amazon = Amazon(paymentMethod)
    amazon.processPayment(amount)
}
```

Now we can define a lot of payment methods and still have it work.

In the upgraded example, we don’t really care about how our method process our payment, but just what it does. We have hide our implementation behind the interface, a contract, that has only significant data, like method name, method arguments and return type. The implementation is than defined on the other side of the contract - consumer.

Abstraction is pretty much everywhere. We use our phones, we know what the power button does but don’t know how. We know that we can turn it off, and it will turn off, but we don’t know how. We just know the important details, in this case that it will turn off. We drive cars, but we don’t know and care about how it works under the hood