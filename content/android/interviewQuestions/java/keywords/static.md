`static` keyword in `Java` is primarily used for memory management. `static` is used to share the same variable or method between all instances of a class. There are some characteristics of the `static` keyword.

- Shared Memory Allocation. `static` variables and methods are allocated in memory exactly once when the program is executed. This memory space is shared among all instances of the class.
- Accessible before any other objects of its class are created.
- Accessible without object instantiation. `static` variables and methods belong to the `class` itself, rather than the instance. It means that you can access `static` variable or method without instantiating the object by juts referring to the class. It also means that changes to the `static` variables for example are reflected to all instances.
- Cannot access non-static members. `static` variables or methods cannot call non-static members. So `static` method cannot call non-static methods. But non-`static` method can call `static` methods.
- Can be overloaded, but can’t be overridden. `static` methods can be overloaded, because `method overloading` is compile-time thing, and they cannot be overridden because `method overriding` happens at runtime.

## `static` usages

### `static` block

If you need to do something when the class is first loaded, e.g. initialize your variables, you can use `static` block to do so.

```java
class Test {
    static int a = 10;
    static int b;

    static {
        System.out.println("Static block initialized.");
        b = a * 4;
    }

    public static void main(String[] args) {
        System.out.println("from main");
        System.out.println("Value of a : " + a);
        System.out.println("Value of b : " + b);
    }
}
```

### `static` variable

If you need to have exactly one copy of a variable and have it linked to the `class`itself, you can use `static` to do so.

```java
class Test {
    static int a;

    static {
        a = 20;
        System.out.println("Inside static block");
    }

    public static void main(String[] args) {
        System.out.println("Value of a : " + a);
        System.out.println("from main");
    }
}

...

class Main {

    public static void main(String[] args) {
        System.out.println(Test.a); // Output: 20
        Test.a = 50;
        System.out.println(Test.a); // Output: 50
    }
}
```

### `static` method

So `static` method comes with some restrictions:

- They can only directly call other `static` methods.
- They can only directly operate on `static` data.
- They cannot refer to `this` or `super` in any way.

However, like a variable, it is now linked to the class itself and can be used without instantiating the class.

```java
class Test {
    static int a = 10;
    int b = 20;

    static void m1() {
        a = 20;
        System.out.println("from m1");

        // Error: Cannot make a static reference to the non-static field b
        b = 10; 

        // Error: Cannot make a static reference to the
        // non-static method m2() from the type Test
        m2();

        //  Error: Cannot use super in a static context
        System.out.println(super.a); // compiler error
    }

    void m2() {
        System.out.println("from m2");
    }

    public static void main(String[] args) {
    }
}
```

### `static` class

If you want to make nested class access only static members of the outer class, mark it `static`. Top-level `class` cannot be marked `static`.

```java
class OuterClass {
 
    // Input string
    private static String msg = "GeeksForGeeks";
 
    // Static nested class
    public static class NestedStaticClass {
 
        // Only static members of Outer class
        // is directly accessible in nested
        // static class
        public void printMessage()
        {
 
            // Try making 'message' a non-static
            // variable, there will be compiler error
            System.out.println(
                "Message from nested static class: " + msg);
        }
    }
 
    // Non-static nested class -
    // also called Inner class
    public class InnerClass {
 
        // Both static and non-static members
        // of Outer class are accessible in
        // this Inner class
        public void display()
        {
 
            // Print statement whenever this method is
            // called
            System.out.println(
                "Message from non-static nested class: "
                + msg);
        }
    }
}
 
// Class 2
// Main class
class GFG {
 
    // Main driver method
    public static void main(String args[])
    {
 
        // Creating instance of nested Static class
        // inside main() method
        OuterClass.NestedStaticClass printer
            = new OuterClass.NestedStaticClass();
 
        // Calling non-static method of nested
        // static class
        printer.printMessage();
 
        // Note: In order to create instance of Inner class
        //  we need an Outer class instance
 
        // Creating Outer class instance for creating
        // non-static nested class
        OuterClass outer = new OuterClass();
        OuterClass.InnerClass inner
            = outer.new InnerClass();
 
        // Calling non-static method of Inner class
        inner.display();
 
        // We can also combine above steps in one
        // step to create instance of Inner class
        OuterClass.InnerClass innerObject
            = new OuterClass().new InnerClass();
 
        // Similarly calling inner class defined method
        innerObject.display();
    }
}
```

From the code, we can see that our `NestedStaticClass` can access only static members of the class, but `InnerClass` can access all the members.