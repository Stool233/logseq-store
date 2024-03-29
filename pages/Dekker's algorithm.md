- **TAG:** [[Computer Science]] [[algorithm]]  [[mutual exclusion]]
- https://en.wikipedia.org/wiki/Dekker%27s_algorithm
- TLA+(PlusCal): ![dekker.pdf](../assets/dekker_1662279942518_0.pdf)
- Dekker's algorithm is the first known correct solution to the [[mutual exclusion]] problem in concurrent programming where processes only communicate via shared memory.
- 主要思想：
	- 定义
		- 通过flag = [thread -> false/true]表示进程此时有意访问CS
		- 通过next_thread表示下一个可以访问cs的进程
	- 算法中，每个进程当发现next_thread不为自己时，会去判断是否有其他进程也有意访问CS，
		- 如果有，则此时先退避，也就是将flag设为false，然后循环等待next_thread为自己
			- 等待next_thread为自己后，重新设置flag为true
		- 如果没有，则可访问CS
	- 算法中，当进程访问CS完成后，则选择其他一个进程作为next_thread，然后将flag设置为false
- Dekker's algorithm guarantees mutual exclusion, freedom from deadlock, and freedom from starvation.
	- 在两个线程的情况下可以满足 保证互斥、不死锁、不饥饿
- One advantage of this algorithm is that it doesn't require special test-and-set (atomic read/modify/write) instructions and is therefore highly portable between languages and machine architectures.
- One disadvantage is that it is limited to two processes and makes use of busy waiting instead of process suspension. (The use of busy waiting suggests that processes should spend a minimum amount of time inside the critical section.)
- 注意CPU、编译器等的特殊优化，对算法正确执行的影响
	- Many modern CPUs execute their instructions in an out-of-order fashion; even memory accesses can be reordered (see memory ordering). This algorithm won't work on SMP machines equipped with these CPUs without the use of memory barriers.
	- Additionally, many optimizing compilers can perform transformations that will cause this algorithm to fail regardless of the platform.
	- To alleviate this problem, volatile variables should be marked as modifiable outside the scope of the currently executing context.
		- Note however that the C/C++ "volatile" attribute only guarantees that the compiler generates code with the proper ordering; it does not include the necessary memory barriers to guarantee in-order execution of that code.
		- C++11 atomic variables can be used to guarantee the appropriate ordering requirements — by default, operations on atomic variables are sequentially consistent so if the wants_to_enter and turn variables are atomic a naive implementation will "just work". Alternatively, ordering can be guaranteed by the explicit use of separate fences, with the load and store operations using a relaxed ordering.
- The other problem with Dekker’s Algorithm is that it’s not resilient. If either thread crashes, it will prevent the other from finishing.
	- 该算法不具备弹性
- some proof: https://itunesu-assets.itunes.apple.com/itunes-assets/CobaltPublic5/v4/17/31/66/173166f4-552e-e5f7-f9df-a5e9843048a8/301-5284704595466591750-CoSc450_Lecture_45_slides.pdf
-