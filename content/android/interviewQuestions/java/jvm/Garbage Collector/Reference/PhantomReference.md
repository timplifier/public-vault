`PhantomReference` is one of 4 types of [[Reference]]. A kind of like last resort for the [[Object]] before is collected. As long as `PhantomReference` is present in [[ReferenceQueue]], the [[Object]] will not be removed. The [[Object]] will be collected only when `PhantomReference` has called `clear()` method. `PhantomReference` is the weakest [[Reference]]. It is important to note that getting the referent via parent `get()` method is not possible, it always returns `null`, thus it justifies why we need [[ReferenceQueue]] to work with `PhantomReference`

## Use Case
There are two reasons to use `PhantomReference`: 
- To determine whether the [[Object]] was removed from the [[Memory]], which helps to schedule memory-sensitive tasks. 
- To avoid using `finalize()` method and improve finalization process.

Usually, scheduling memory-sensitive tasks is accomplished by creating your own object that inherits from `PhantomReference`

```java
public class LargeObjectFinalizer extends PhantomReference<Object> {

    public LargeObjectFinalizer(
     Object referent, ReferenceQueue<? super Object> q) {
        super(referent, q);
    }

    public void finalizeResources() {
        System.out.println("clearing ...");
    }
}
```

```java
ReferenceQueue<Object> referenceQueue = new ReferenceQueue<>();
List<LargeObjectFinalizer> references = new ArrayList<>(); 
List<Object> largeObjects = new ArrayList<>(); 
for (int i = 0; i < 10; ++i) { 
	Object largeObject = new Object(); largeObjects.add(largeObject); 
	references.add(new LargeObjectFinalizer(largeObject, referenceQueue)); 
} 

largeObjects = null; 
System.gc(); 
Reference<?> referenceFromQueue; 
for (PhantomReference<Object> reference : references) { 
	System.out.println(reference.isEnqueued()); 
} 

while ((referenceFromQueue = referenceQueue.poll()) != null) { 
	((LargeObjectFinalizer)referenceFromQueue).finalizeResources(); 
	referenceFromQueue.clear(); 
}
```
