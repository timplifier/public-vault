When we create an [[Object]], do we even think about how it is then deconstructed ? No, we don't even think about it, because [[Garbage Collector]] does the work for us. Not specifying any reference in most of the cases is the best option, but sometimes, we would like to control when it is available for destruction.

There are essentially 4 types of references (in descending order):
- [[Strong (Hard) Reference]]
- [[SoftReference]]
- [[WeakReference]]
- [[PhantomReference]]

```mermaid
flowchart LR 
StrongReference-->SoftReference
SoftReference-->WeakReference
WeakReference-->PhantomReference

