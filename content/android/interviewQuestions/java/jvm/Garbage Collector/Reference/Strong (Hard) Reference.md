A default type of [[Reference]] and you have been using it for a really long time from the very first day you started using [[Java]]. I think `Strong Reference` can be described in this manner:
**The [[Object]] can't be garbage collected if it's reachable through any strong reference**. 
In other words, unless it is `null`, it **can't be garbage collected**. So only assigning `null` to the variable, just like this

```java
SomeClass obj = new SomeClass();
obj = null;
```

In this case, `obj` points to `SomeClass` instance in the [[Heap]], and `SomeClass` is gonna persist in the [[Heap]] as long as it is reachable through any strong reference, in our case it is `obj`. But the moment when we assign `null` to the `obj` variable, it no longer points to `SomeClass`, therefore `SomeClass` has no active references to it and it is eligible for garbage collection.

## Use Case
`Strong (Hard) Reference` has been selected as default reference because it allows [[Garbage Collector]] to behave as it was designed to: **Garbage Collector is responsible for Memory Management, not the Developer himself**. Under most of the circumstances, you can just use `Strong (Hard) Reference` and it will work just fine.