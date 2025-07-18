`SoftReference` is one of 4 types of [[Reference]]. It tells the [[Garbage Collector]] that a referenced [[Object]] can be destructed when [[JVM]] runs low on memory. The Object can stay in memory as long as [[JVM]] has some free space to work with. **All soft references to objects reachable only by soft reference should be cleared out before the `OutOfMemoryError` exception is thrown.**

`SoftReference` is eligible for collection only and only if:
- The referent is not strongly reachable.
- The `SoftReference` itself hasn't been accessed recently.
So `SoftReference` can persist in memory for hours after the referent becomes unreachable. [[WeakReference]] on the other hand, will be available only for as long as its referent is still around

We can use `SoftReference` easily by just wrapping it around the Object:

```java
import java.util.ref.SoftReference
import java.util.List

SoftReference<List<String>> listReference = new SoftReference<>(new ArrayList<String>());
```

In order to retrieve the object we refer to using `SoftReference`, just call `get()` method.

```java
List<String> list = listReference.get();
if (list == null) {
	// Object was already garbage collected
}
```

## Use Case
`SoftReference` can be used in critical sections of the application where errors related to insufficient memory can occur, so our Object can persist in memory as long as `JVM` has sufficient memory. Unlike [[Strong (Hard) Reference]], [[JVM]] can decide whether to destruct Object from the [[Heap]] or not if it runs out of memory.