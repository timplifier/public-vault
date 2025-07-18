In C/C++, a developer is responsible for full memory management, including both constructing and destructing [[Object]]s, however, destructing [[Object]]s is often neglected so at some point in the app there will be no sufficient amount of memory to work with and therefore application will be terminated. But one day, Java changed everything by introducing Garbage Collector

Even pretty much light JVM application instantiates a lot of [[Object]]s, Arrays, manipulate them, modify them, reassign them and much more. More and more memory is allocated, and it would be obvious if our program would start throttling, lagging due to running out of space, however, it never really happens, doesn't it? Developers don't even notice the process of memory deallocation. And only few wonder why it is like that. All of it is handled by Garbage Collector. 

**Garbage Collector** is the process of destroying [[Object]]s that allocate memory but are not used anywhere in the application at the present moment. To put it simple, **Garbage Collector** looks through the [[Objects]]s that are no longer referenced from the [[Stack]], but are present in the [[Heap]].

Basically, there are two types of [[Object]]s that Garbage Collector knows:

- Young Generation. Every [[Object]] that is instantiated for the first time comes here. Garbage Collector performs `Mark and Sweep` process on Young Generation often and at first - marks [[Object]]s for removal that are unreferenced in the application and at second - removes them. Those that are not marked are moved to the next - Old Generation.
- Old Generation - Every [[Object]] that survived Young Generation is moved here. Garbage Collection happens less frequently and those are treated as application-scoped [[Object]]s.

## Garbage Collection Eligibility
Let's have a look at the following code:

```java
Integer i = new Integer(4);
// the new Integer object is reachable  via the reference in 'i' 
i = null;
// the Integer object is no longer reachable.
```
As we can see, we assigned `null` to the `i` variable and because there is now no reference to the `i`, in other words no pointer, it is considered eligible for GC(garbage collection).

There are multiple ways to make an [[Object]] eligible for GC:
1. Nullifying the reference variable. Assigning  `null` value to the variable as we did above.
2. Re-assigning the reference variable. If reference variable is reassigned, it is pointing to the other [[Object]]
```java
Integer i = new Integer(4);
// the new Integer object is reachable  via the reference in 'i' 
i = 4;
// the Integer object with reference 'i' and value '5' is created
i = 5;
// the Integer object with value "4" is no longer reachable.
```
3. An [[Object]] is created inside the method and referenced only in the method.
```java
public void foo() {
	MyObject obj = new MyObject();
}
```
`obj` is the reference  from [[Stack]] to the `MyObject` that is contained in [[Heap]]. When function returns, `foo` function is gonna be popped off the [[Stack]], therefore `obj` reference is destroyed and because the reference to `MyObject` was only from the `foo` method, `MyObject` is deallocated.
4. Island of Isolation
```java
class A {
	B b; 
}
class B { 
	A a; 
}
public void createIsland() {
	A a = new A();
	B b = new B();
	a.b = b;
	b.a = a;
}
```

So `A` and `B` reference each other. in `createIsland` after the allocation of `A` & `B` in the [[Heap]], if neither of them is not reachable after `createIsland`, they are eligible for GC, but if at least one of them is referenced, none of them is eligible for GC. This is called `Island of Isolation` because they reference each other but not reachable from any live [[Thread]] or root reference.

## Manual Garbage Collection Call
If we make [[Object]] eligible for GC, it may not be destroyed immediately, only when JVM runs Garbage Collection. But we don't know when it would run it. 
We can request JVM to run Garbage Collection in two ways:
1. `System.gc()`. It is a static method for requesting JVM to run GC.
2. `Runtime.getRuntime().gc()`. Effectively the same as `System.gc()`

However, there is no guarantee that GC will run after the request


## Finalization
Just before destroying the [[Object]], GC calls `finalize()` method on the object to perform cleanup activities. `finalize()` method is present in [[Object]] class with the following signature:
```java
protected void finalize() throws Throwable
```
We can override `finalize()` method for performing cleanup activities
Important notes about `finalize()`.

1. The `finalize()` method is called by GC, not [[JVM]]. However, GC is part of [[JVM]].
2. `finalize()` method has an empty implementation, so it is recommended to override it if you need to cleanup something.
3. `finalize()` method is never invoked more than once for any [[Object]]
4. `finalize()` ignores uncaught [[Exception]]. If `finalzie()` method throws [[Exception]], the [[Exception]] is ignored and thus doesn't affect GC process of memory deallocation.

If you are not familiar with either [[Stack]] or [[Heap]],  feel free to check out [[Memory]]