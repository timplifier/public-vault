`ReferenceQueue` is a simple data structure that allows you to pass a [[Reference]] to it in order to track when the [[Object]] is cleaned. `ReferenceQueue` could be used to find out when Object's reference is [[SoftReference]], [[WeakReference]] or [[PhantomReference]] 

`ReferenceQueue` is added to the reference by constructor.

```java
import java.lang.ref.ReferenceQueue;
import java.lang.ref.SoftReference;
import java.util.ArrayList;
import java.util.List;

List<String> list = new ArrayList<>();
ReferenceQueue<List<String>> listReferenceQueue = new ReferenceQueue<>();
SoftReference<List<String>> listSoftReference = new SoftReference<>(list, listReferenceQueue);
```

| Method name            | Method description                                                                                                                                                                                     |
| :--------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `poll()`               | Polls this queue to see if a reference object is available. If one is available without further delay then it is removed from the queue and returned. Otherwise, this method immediately returns null. |
| `remove()`             | Removes the next reference object in this queue, blocking until one becomes available.                                                                                                                 |
| `remove(long TimeOut)` | Removes the next reference object in this queue, blocking until either one becomes available or the given timeout period expires.                                                                      |
## Use Case

`ReferenceQueue` methods allows us to take some action when our Object reachability weakens. So we can perform some post-finalization cleanup when Object becomes phantom reachable.