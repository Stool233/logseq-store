- TLA+的底层基本抽象
	- An execution of a system is represented as a sequence of discrete steps
		- 一个系统的执行被表示为一系列离散的步骤
		- sequence
		- discrete
			- Digital system
				- We can abstract its continuous evolution as a sequence of discrete events
					- 我们可以把它的 连续演变 抽象为一系列 离散事件
		- step
			- TLA+ describe a step as a state change
				- 状态变化
				- An execution is represented as a sequence of states
				- A step is the change from one state to the next
			- Science models systems by a state changing with time, usually continuously
				- 科学通过一种随时间变化的状态(通常是连续的)来模拟系统
			- As in science, TLA+ describes a state as an assignment of values to variables
				- 在科学中，TLA+将状态描述为对变量赋值
- 状态机（state machine）
	- An execution of a system is represented as a behavior
	- A behavior is a sequence of states
	- We want to specify all possible of a system
	- State Machine
		- A state machine is described by
			-
			  1. All possible initial states
			-
			  2. What next states can follow any given state
		- It halts if there is no possible next state.
		- A state is an assignment of values to variables, so a state machine is described by
			-
			  0. What the variables are.
			-
			  1. Possible initial values of variables.
			-
			  2. A relation between their values in the current state and their possible values in the next state
		-
		-