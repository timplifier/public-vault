Is a typical Map where each type is represented as [[Element]] and has its own unique [[Key]]. CoroutineContext can be merged using `plus` operator.

```mermaid
classDiagram  
direction LR
class CoroutineContext1
    CoroutineContext1: Job
    CoroutineContext1: Dispatcher
	CoroutineContext1: 

class CoroutineContext2
	CoroutineContext2: Name
	CoroutineContext2: Job
	CoroutineContext2: Dispatcher

class CoroutineSum 
	CoroutineSum: Name
	CoroutineSum: Job
	CoroutineSum: Dispatcher

CoroutineContext1 .. CoroutineContext2
CoroutineContext2 ..|> CoroutineSum
```

Dispatchers are gonna be merged, values that have same [[Key]], gonna be taken from right. It is useful when you want to combine multiple [[Job]] instances