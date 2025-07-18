A weakly referenced [[Object]] is cleared by the [[Garbage Collector]] when it’s `weakly reachable`.

Weak reachability means that neither [[Strong (Hard) Reference]] nor [[SoftReference]] point to it.

[[Garbage Collector]] clears a weak reference so the referent is no longer accessible. Then the [[Reference]] is placed in a [[ReferenceQueue]] (if any associated exists) and at the same time, formerly weakly-reachable objects are going to be finalized.

## Use Case
`WeakReference` ultimately has the following use cases:
- To implement canonicalizing mapping. A mapping is called `canonicalizing` if it holds only one instance of a particular value. Instead of creating a new [[Object]], it will lookup for the existing one in the mapping and uses it.
- [[ Lapsed Listener Problem]]]]. Essentially, it is a problem where publisher holds [[Strong (Hard) Reference]] to all the subscribers, but because of that these subscribers can't unsubscribe from a publisher.

`WeakReference` can be used like that:

```java
Object referent = new Object();
ReferenceQueue<Object> referenceQueue = new ReferenceQueue<>();

WeakReference weakReference1 = new WeakReference<>(referent);
WeakReference weakReference2 = new WeakReference<>(referent, referenceQueue);
```

The referent of the [[Reference]] can be obtained by calling `get()` and cleared by calling `clear()` method.

```java
Object referent2 = weakReference1.get();
weakReference1.clear();
```

So pattern is kind of similar to [[SoftReference]]
```java
Object referent3 = weakReference2.get();
if (referent3 != null) {
    // GC hasn't removed the instance yet
} else {
    // GC has cleared the instance
}
```
