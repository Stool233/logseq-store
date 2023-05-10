- **TAG: [[Computer Science]] **
- 相关文章
	- Let's talk locks
		- 来源：
			- https://www.infoq.com/presentations/go-locks/
		- let’s build a lock! (a tour through lock internals)
			- our case-study
				- Lock implementations are hardware, ISA, OS and language specific:
					- We assume an **x86_64 SMP machine** running a **modern Linux**.
						- SMP System
							- ![image.png](../assets/image_1683719235930_0.png){:height 263, :width 375}
					- We’ll look at the lock implementation in **Go 1.12**.
						- a brief go primer
							- The unit of concurrent execution: goroutines.
								- use as you would threads
								- but user-space threads:
									- managed entirely by the Go runtime, not the operating system.
								- Data shared between goroutines must be synchronized.
								- One way is to use the blocking, non-recursive lock construct:
									- ```
									  ```
							-
					-
		- let’s analyze its performance! (performance models for contention)
		- let’s use it, smartly! (a few closing strategies)
			-