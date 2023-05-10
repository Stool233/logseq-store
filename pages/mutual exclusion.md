- **TAG: * [[Computer Science]] [[locks]]
- Dijkstra's Mutual Exclusion Problem（Dijkstra的互斥问题）
	- http://rust-class.org/class-20-mutual-exclusion.html
	- Requirements （它要求）
		- Only one thread may be in the critical section at any time.
		- Each must eventually be able to enter its critical section.
		- Must be symmetrical (all run same program).
		- Cannot make any assumptions about speed of threads.
		- ![image.png](../assets/image_1670250795719_0.png)
	- 一些不可靠的解法
		- 问题一般出在一些操作有原子性的假设，但该假设实际不成立
		  collapsed:: true
			- ![image.png](../assets/image_1670251931583_0.png)
			- ![image.png](../assets/image_1670251946876_0.png)
	- 内核上的简单解法
		- 通过disable中断
		  collapsed:: true
			- ![image.png](../assets/image_1670252089089_0.png)
			- ![image.png](../assets/image_1670252096697_0.png)
			- ![image.png](../assets/image_1670252110389_0.png)
	- 硬件解法
		- test and set
		  collapsed:: true
			- ![image.png](../assets/image_1670252165743_0.png)
			- ![image.png](../assets/image_1670252183544_0.png)
			- ![image.png](../assets/image_1670252202100_0.png)
			-
		- 其他细节见网站
	- Dijkstra的解法
		-