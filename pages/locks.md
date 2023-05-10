- **TAG: [[Computer Science]] **
- 相关文章
	- Let's talk locks
		- 来源：
			- https://www.infoq.com/presentations/go-locks/
		- our case-study
		  collapsed:: true
			- Lock implementations are hardware, ISA, OS and language specific:
				- We assume an **x86_64 SMP machine** running a **modern Linux**.
					- SMP System
						- ![image.png](../assets/image_1683719235930_0.png){:height 263, :width 375}
				- We’ll look at the lock implementation in **Go 1.12**.
					- a brief go primer
						- The unit of concurrent execution: goroutines.
							- use as you would threads
								- ```go
								  go handle_request(r)
								  ```
							- but user-space threads:
								- managed entirely by the Go runtime, not the operating system.
							- Data shared between goroutines must be synchronized.
							- One way is to use the blocking, non-recursive lock construct:
								- ```go
								  var mu sync.Mutex
								  mu.Lock()
								  ...
								  mu.Unlock()
								  ```
				-
				-
		- let’s build a lock! (a tour through lock internals)
			- want: “[[mutual exclusion]]”
				- only one thread has access to shared data at any given time
			- use a flag?
			  collapsed:: true
				- ![image.png](../assets/image_1683720086369_0.png)
				- ![image.png](../assets/image_1683720101428_0.png)
				- ![image.png](../assets/image_1683720111922_0.png)
			- atomicity
				- A memory operation is **non-atomic** if it can be observed half-complete by another thread.
				- An operation may be non-atomic because it:
					- uses multiple CPU instructions:
						- operations on a large data structure;
						- compiler decisions.
					- uses a single non-atomic CPU instruction:
						- RMW instructions; unaligned loads and stores.
						  collapsed:: true
							- ```
							  flag++
							  ```
				- An **atomic operation** is an “indivisible” memory access.
				- In x86_64, loads, stores that are   naturally aligned up to 64b.*（在x86_64中，加载、存储 自然对齐到64b）
					- guarantees the data item fits within a cache line;（保证数据项适合缓存行）
					- **cache coherency** guarantees a consistent view for a single cache line.（缓存一致性保证了单个缓存行的一致视图）
			- use a flag? nope; not atomic.
				- ![image.png](../assets/image_1683720708455_0.png){:height 289, :width 222}
					- the compiler may reorder operations.
					  collapsed:: true
						- ![image.png](../assets/image_1683720721888_0.png)
					- the processor may reorder operations.
					  collapsed:: true
						- ![image.png](../assets/image_1683720755408_0.png)
			- memory access reordering
				- The compiler, processor can **reorder memory operations** to optimize execution.
					- The only cardinal rule is **sequential consistency for single threaded programs.**
					- Other guarantees about compiler reordering are captured by a   **language’s memory model**:
						- C++, Go guarantee data-race free programs will be sequentially consistent.（C++、Go保证没有数据竞争的程序将是顺序一致的。）
					- For processor reordering, by the **hardware memory model**:
						- x86_64 provides Total Store Ordering (TSO).
							- a relaxed consistency model.
							- most reorderings are invalid but StoreLoad is game;  allows processor to hide the latency of writes.
							  collapsed:: true
								- (这是一种相对较弱的一致性模型，虽然大多数的重排序都是无效的，但是对于存储-加载这种操作是有效的。这意味着处理器可以在隐藏写入操作的延迟的同时，对其他指令进行重排序，以提高执行效率。)
									- "无效"的重排序是指处理器对指令的执行顺序进行的重排序，不会改变程序的行为，即不会导致程序的正确性受到影响。在这种情况下，重排序是被允许的。
									- "有效"的重排序是指处理器对指令的执行顺序进行的重排序，可能会导致程序的行为发生变化，即可能会导致程序的正确性受到影响。在这种情况下，重排序是不被允许的，需要通过内存模型的规则来限制。
			- use a flag? nope; not atomic and no memory order guarantees.
				- need a construct that provides atomicity and prevents memory reordering.
					- ...the hardware provides!
			- special hardware instructions
				- For **guaranteed atomicity** and to **prevent memory reordering**.
					- **guaranteed atomicity**
						- x86 example: XCHG (exchange)
					- **prevent memory reordering**
						- these instructions are called memory barriers.
						- they prevent reordering by the compiler too.
						- x86 example: MFENCE, LFENCE, SFENCE.
					- The x86 LOCK instruction prefix provides **both**.
					-
		- let’s analyze its performance! (performance models for contention)
		- let’s use it, smartly! (a few closing strategies)
			-