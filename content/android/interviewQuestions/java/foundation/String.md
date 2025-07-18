String is a kind of like common class in [[Java]] that is considered primitive. However,  [[Primitive]] in Java starts with the lowercase and cannot be instantiated with the `new` keyword. However, String is an exception. String **does not start with the lowercase**, can be instantiated with both `new` keyword and without it. Everything can be cast to String, because everything is a character.

## Immutability
String is immutable, but why is that ? Take a look at this piece of code:
```java
String name = "John";
name = "Larl";
```

We can see here that we reassign the value of `name` reference, so it should be just changed like a [[Primitive]], however, `String` is an [[Object]], so new instance is created in [[Heap]] with the value `Larl`, so String with the value `John` still exists in the String Pool. 

## String Pool (String Intern Pool/String Constant Pool)
Look at the following code:

```java
String name = "John";
String surname = "John";
String middleName = "John";
```

If thinking that `String` is an [[Object]], there should be three separate [[Object]]s with the same value, just like with the classes. However, JVM is smart enough to notice that they all have the same value representation, so as you might guess, only one instance of `String` is created in the `String Pool`, and before `String` is added to the `String Pool`, it is first looked up in the `String Pool` itself in order to check if its value representation already exists. It allows [[JVM]] to save a lot of memory. Measurements have shown that **roughly 25% of the JVM [[Heap]] space is consumed by `String` objects**, which is quite a lot, so imagine that if JVM wouldn't have `String Pool?`. So `String Pool` is the special area in [[Heap]] space used specifically for `Strings`. 

```java
public static void main(String[] args) {
        String s1 = "Java";
        String s2 = "Java";
        String s3 = new String("Java");
        String s4 = new String("Java").intern();
		/*
         * true, have same references because they are in `String Pool`
         */
        System.out.println(s1 == s2); 
        /*
         * false, `s1` is in the `String Pool` and `s3` is in the `Heap`
         */
        System.out.println(s1 == s3); 
        /*
         * true, `s1` is in the `String Pool` and `s4` is also in the `String Pool` because of `intern()`
         */
        System.out.println(s1 == s4); 
```

If you run the code above, you will see that though all the `Strings` looks equal, **they are actually not**, because `==` compares by reference, not the value and we can see that we will get `false` when `String` from `String Pool` and [[Heap]] is compared, because they have different references though have the same value, but when we check our third test-case, we can see  that the result is true, but how is that ? `s4` is created using `new` keyword, so it should be placed in the [[Heap]], right ? The answer is: not always.

You can put `String` in the `String Pool` by doing the following:
1. String literals. Strings that are not created using `new`.
```java
// Put into String Pool
String name = "John";
// Put into Heap
String surname = new String("Johnson");
```
2. Using `intern()` method. `intern()` method stores `String` in the `String Pool`.
```java
// Put into Heap
String name = new String("John");
// Puut into String Pool
String surname = new String("John").intern();
```
3. Constant Expressions.
```java
String s4 = "Java" + "is the best";
```
4. Strings used in [[switch]] statements. When strings are used as the expression in a switch statement, the compiler ensures that the `String Pool` is utilized to optimize comparison efficiency.
```java
    public static void main(String[] args) {
        String input = "apple";

        switch (input) {
            case "apple":
                System.out.println("You selected apple.");
                break;
            case "banana":
                System.out.println("You selected banana.");
                break;
            case "cherry":
                System.out.println("You selected cherry.");
                break;
            default:
                System.out.println("Unknown fruit.");
        }
    }
```
5. String Constants in [[interface]]. If an `interface` defines `String` constant, it is added to the `String Pool`.
```java
interface Java {
String COMPANY = "Oracle";
}
```

`Strings` from the `String Pool` can't be destructed and collected by [[Garbage Collector]]