- **TAG**: [[Computer Science]] [[形式化证明]]
- TLA+ Video Course 的整理
	- https://lamport.azurewebsites.net/video/videos.html
- [[TLA+]]的底层基本抽象
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
- [[状态机]]（[[state machine]]）
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
				- 变量是什么
			-
			  1. Possible initial values of variables.
				- 变量可能的初始值。
			-
			  2. A relation between their values in the current state and their possible values in the next state
				- 它们在当前状态下的值与它们在下一状态下可能的值之间的关系
	- State machine are simpler than programs
		- In a state machine, all part of the state are represented as value of variables
			- 在状态机中，状态的所有部分都表示为变量的值
		- In programs, different parts of the state are represented differently
			- example
				- The value of variables
				- The control state
				- The call stack
				- The heap
				- ...
			- They are represented differently because they are implemented differently
				- State machine eliminate those low-level implementation details
					- They provide a single simple abstraction
	- TLA+ is an elegant, expressive language for describing state machine.
- Describing a state machine with math (用数学描述状态机)
	- 演进
	  collapsed:: true
		- ![image.png](../assets/image_1654529341200_0.png){:height 274, :width 397}
		- ![image.png](../assets/image_1654529370480_0.png)
		- ![image.png](../assets/image_1654529400161_0.png){:height 322, :width 378}
		- ![image.png](../assets/image_1654529474705_0.png){:height 299, :width 502}
		- ![image.png](../assets/image_1654529556431_0.png){:height 365, :width 391}
		- ![image.png](../assets/image_1654529592063_0.png){:height 336, :width 393}
		- ![image.png](../assets/image_1654529639541_0.png)
			- The big difference between math and C :
				- Math is much more expressive
				- Programming languages weren't designed to express nondeterminism (编程语言不是为了表达非确定性而设计的)
					- They lack more than constructs for nondeterminism
						- 他们缺乏的不仅仅是对非确定性的构想
					- Programming languages don't abstract above the code level
						- 他们不会在代码层之上进行抽象
	- 记住转换后的这个是数学公式（formula），符合交换律等数学性质
	  collapsed:: true
		- ![image.png](../assets/image_1654529983447_0.png)
		- ![image.png](../assets/image_1654530003980_0.png)
	- TLA+ 完整例子
	  collapsed:: true
		- ![image.png](../assets/image_1654530134394_0.png)
		- ![image.png](../assets/image_1654530152962_0.png)
- Decomposing large specs
	- Using definitions（通过定义，来模块化，减少specs理解成本）
	  collapsed:: true
		- ![image.png](../assets/image_1654530220178_0.png)
		- ![image.png](../assets/image_1654530284057_0.png)
- Why Does TLC Report Deadlock
	- Deadlock
		- Execution stopped when it wasn't supposed to.
	- Termination
		- Execution stopped when it was supposed to.
- The TLA+ proof system
	- TLA+ has constructs for writing theorems and formal proofs of those theorems.
		- TLA+构造了定理和这些定理的形式证明。
	- TLAPS is tool for checking those proofs.
-
- Getting Started on a Spec
	- The best way
		- Write a single correct behavior. Informally
-
- In TLA+, every value is a set, But TLA+ doesn't say what their elements are.
- The TLA+ syntax for an array expression
  collapsed:: true
	- ![image.png](../assets/image_1654612447477_0.png)
- TLA+ Terminology
  collapsed:: true
	- ![image.png](../assets/image_1654612529719_0.png){:height 328, :width 480}
	- Many popular programming languages allow only index sets 0 .. n
	- Math and TLA+ allow a function to have any set as its domain -- for example, the set of all integers.
- [[Transaction Commit]]
  collapsed:: true
	- ![image.png](../assets/image_1654615349995_0.png){:height 485, :width 442}
	- ![image.png](../assets/image_1654613325201_0.png){:height 353, :width 468}
	- ![image.png](../assets/image_1654614341996_0.png){:height 163, :width 499}
- Records
  collapsed:: true
	- ![image.png](../assets/image_1654617335258_0.png)
	- ![image.png](../assets/image_1654617393371_0.png)
	- ![image.png](../assets/image_1654617425652_0.png)
	- ![image.png](../assets/image_1654617456634_0.png){:height 197, :width 496}
- Two-Phase Commit
	- A popular algorithm for implementing transaction commit
	- Send Messages Spec
		- The spec must describe sending messages.
		- It should specify only what's required of message passing.
		- A simple method
			- Let *msgs* be the set of messages currently in transit
		- A simpler method
			- Let *msgs* be the set of all messages ever sent.
			- A single message can be received by multiple processes.
			- A process can receive the same messages multiple times.
			- Two-Phase commit still works
	- Spec
	  collapsed:: true
		- ![image.png](../assets/image_1654688771400_0.png)
		- ![image.png](../assets/image_1654688808584_0.png)
		- ![image.png](../assets/image_1654689105828_0.png)
		- ![image.png](../assets/image_1654689294351_0.png)
		- ![image.png](../assets/image_1654689389215_0.png)
		- ![image.png](../assets/image_1654689759657_0.png)
		- ![image.png](../assets/image_1654690693818_0.png)
		- ![image.png](../assets/image_1654690722731_0.png)
		- ![image.png](../assets/image_1654690895760_0.png)
			- After r has aborted, no RM can ever commit; and the TM should eventually take a TMAbort step.
			- In practice, r would inform the TM that it has aborted so the TM knows it should abort the transaction.
			- But the optimization isn't relevant for implementing TCommit.
		- ![image.png](../assets/image_1654691116691_0.png)
		- ![image.png](../assets/image_1654691166876_0.png)
	- Model Values
		- [[Symmetry Sets]]
		  collapsed:: true
			- ![image.png](../assets/image_1654694014794_0.png)
			- ![image.png](../assets/image_1654694057842_0.png)
			- TLC will check fewer states if the model sets a symmetry set to a set of model values.
	- Correctness of Two-Phase
		- Two-phase commit doesn’t just maintain the invariance of TCConsistent.
			- 两阶段提交不仅保持了TCConsistent的不变性。
		- It implements the specification of transaction commit.
			- 它实现了transaction commit的规范。
- [[Paxos]] Commit
	- The problem with two-phase commit:
		- It can hang forever if the TM fails.
	- In math, any expression equals itself.
	- Spec
	  collapsed:: true
		- ![image.png](../assets/image_1654871227194_0.png)
		- ![image.png](../assets/image_1654871457834_0.png)
		- ![image.png](../assets/image_1654871495184_0.png)
		- ![image.png](../assets/image_1654871511038_0.png)
		- ![image.png](../assets/image_1654871571072_0.png)
		- ![image.png](../assets/image_1654871634948_0.png)
		- ![image.png](../assets/image_1654871676962_0.png)
		- ![image.png](../assets/image_1654871783235_0.png)
			- ![image.png](../assets/image_1654871839852_0.png)
		- ![image.png](../assets/image_1654872076386_0.png)
		- ![image.png](../assets/image_1654872109498_0.png){:height 234, :width 594}
		- ![image.png](../assets/image_1654872341718_0.png)
		- ![image.png](../assets/image_1654872397826_0.png)
		- ![image.png](../assets/image_1654872437035_0.png)
		- ![image.png](../assets/image_1654872900145_0.png)
			- The LET Clause makes three definitions local to the LET IN expression
		- ![image.png](../assets/image_1654872884924_0.png){:height 300, :width 682}
			- The defined identifier can be only in the expression
		- ![image.png](../assets/image_1654872997609_0.png)
		- ![image.png](../assets/image_1654873086380_0.png)
		- ![image.png](../assets/image_1654873096250_0.png)
	- it’s ok to use elements of a symmetry set in an expression assigned to another constant if the expression is symmetric in the elements of the symmetry set.
		- 如果表达式在对称集合的元素中是对称的，那么在赋给另一个常数的表达式中使用对称集的元素是可以的。
	- There’s one additional condition for symmetry sets.
		- Elements of a symmetry set, or a constant assigned elements of a symmetry set may not appear in a CHOOSE expression.
			- 对称集还有一个附加条件。
			  对称集的元素，或对称集的常量赋值元素不能出现在CHOOSE表达式中。
	- What good is checking such small models
		- Even a very small model can catch an error in an algorithm
- Implementation
	- Preliminaries
		- Logical Implication
		  collapsed:: true
			- ![image.png](../assets/image_1654876089759_0.png)
			- In speech, implication asserts causality.
			- In math, implication asserts only correlation.
		- Ordinary Expressions
			- Module-Closed Expression
			  collapsed:: true
				- ![image.png](../assets/image_1654876211372_0.png)
					- ![image.png](../assets/image_1654876276278_0.png)
				- ![image.png](../assets/image_1654918438353_0.png)
				- ![image.png](../assets/image_1654918466210_0.png)
				- ![image.png](../assets/image_1654918493124_0.png)
				- ![image.png](../assets/image_1654918573236_0.png)
			- Constant Expressions
			  collapsed:: true
				- ![image.png](../assets/image_1654918612085_0.png)
				- ![image.png](../assets/image_1654918738039_0.png)
			- An assumption
			  collapsed:: true
				- ![image.png](../assets/image_1654918796039_0.png)
			- State Expressions
			  collapsed:: true
				- ![image.png](../assets/image_1654919169379_0.png)
				- ![image.png](../assets/image_1654919295616_0.png)
				- ![image.png](../assets/image_1654919391263_0.png)
				- ![image.png](../assets/image_1654919427869_0.png)
			- Action Expression
			  collapsed:: true
				- ![image.png](../assets/image_1654919504947_0.png)
				- ![image.png](../assets/image_1654919735704_0.png)
			- Priming a state expression
			  collapsed:: true
				- ![image.png](../assets/image_1654919840580_0.png)
		- Temporal formulas
		  collapsed:: true
			- ![image.png](../assets/image_1654920247307_0.png)
			- ![image.png](../assets/image_1654920267539_0.png)
			- ![image.png](../assets/image_1654920354969_0.png)
			- ![image.png](../assets/image_1654921040070_0.png)
			- ![image.png](../assets/image_1654921108139_0.png)
			- ![image.png](../assets/image_1654921212743_0.png)
			- ![image.png](../assets/image_1654923820884_0.png)
			- ![image.png](../assets/image_1654923934655_0.png)
			- ![image.png](../assets/image_1654923958896_0.png)
			- ![image.png](../assets/image_1654924241178_0.png)
			- ![image.png](../assets/image_1654924279784_0.png)
			- ![image.png](../assets/image_1654924312098_0.png)
			- ![image.png](../assets/image_1654924342510_0.png)
			- ![image.png](../assets/image_1654924371850_0.png)
			- ![image.png](../assets/image_1654924446838_0.png)
	- How it works
		- The theorem
		  collapsed:: true
			- ![image.png](../assets/image_1654924733080_0.png)
			- ![image.png](../assets/image_1654924757815_0.png)
			- ![image.png](../assets/image_1654924796990_0.png)
			- ![image.png](../assets/image_1654924845631_0.png)
			- ![image.png](../assets/image_1654924874707_0.png)
			- ![image.png](../assets/image_1654924887658_0.png)
			- ![image.png](../assets/image_1654925118957_0.png)
			- ![image.png](../assets/image_1654925270778_0.png)
			- ![image.png](../assets/image_1654925379406_0.png)
			- ![image.png](../assets/image_1654925421580_0.png)
			- ![image.png](../assets/image_1654925503362_0.png)
			-
		- Stuttering
		  collapsed:: true
			- ![image.png](../assets/image_1654925642812_0.png)
			- ![image.png](../assets/image_1654925815655_0.png)
			- ![image.png](../assets/image_1654925896714_0.png)
			- ![image.png](../assets/image_1654925907644_0.png)
			- ![image.png](../assets/image_1654925948778_0.png)
			- ![image.png](../assets/image_1654925985992_0.png)
			- ![image.png](../assets/image_1654926065329_0.png)
			- ![image.png](../assets/image_1654926123531_0.png)
			- ![image.png](../assets/image_1654926150384_0.png)
			- ![image.png](../assets/image_1654926248343_0.png)
			- ![image.png](../assets/image_1654926275838_0.png)
				- mathematical simplicity is not an end in itself.
				  it’s a sign that we’re doing things right.
					- 数学上的简单性本身并不是目的。
					  这表明我们在做正确的事情。
			-
			-
		- Termination and stopping
		  collapsed:: true
			- ![image.png](../assets/image_1654926919059_0.png)
			- ![image.png](../assets/image_1654926956816_0.png)
			- ![image.png](../assets/image_1654926973068_0.png)
			- ![image.png](../assets/image_1654927026820_0.png)
			- ![image.png](../assets/image_1654927068657_0.png)
			- ![image.png](../assets/image_1654927101153_0.png)
			- ![image.png](../assets/image_1654927126780_0.png)
			-
			-
			-
			-
- The Alternating Bit Protocol
	- Finite sequence 
	  collapsed:: true
		- ![image.png](../assets/image_1654936391049_0.png)
		- ![image.png](../assets/image_1654936435559_0.png)
		- ![image.png](../assets/image_1654936481741_0.png)
		- ![image.png](../assets/image_1654936549627_0.png)
		- ![image.png](../assets/image_1654936566031_0.png)
		- ![image.png](../assets/image_1654936596780_0.png)
		- ![image.png](../assets/image_1654936726125_0.png)
	- The Cartesian Product
	  collapsed:: true
		- ![image.png](../assets/image_1654937170068_0.png)
		- ![image.png](../assets/image_1654937182555_0.png)
		- ![image.png](../assets/image_1654937301975_0.png)
	- What the protocol should accomplish
	  collapsed:: true
		- ![image.png](../assets/image_1654937588023_0.png)
		- ![image.png](../assets/image_1654937631243_0.png)
		- ![image.png](../assets/image_1654937663290_0.png)
		- ![image.png](../assets/image_1654937678975_0.png)
		- ![image.png](../assets/image_1654937690919_0.png){:height 87, :width 500}
		- ![image.png](../assets/image_1654937894949_0.png)
		-
		-
	- Safety and liveness
	  collapsed:: true
		- [[Safety Formula]]
		  collapsed:: true
			- ![image.png](../assets/image_1654938318253_0.png)
			- ![image.png](../assets/image_1654938409956_0.png)
			- ![image.png](../assets/image_1654938441351_0.png)
		- [[Liveness Formula]]
		  collapsed:: true
			- ![image.png](../assets/image_1654938530296_0.png)
			- ![image.png](../assets/image_1654938575381_0.png)
			- ![image.png](../assets/image_1654938734492_0.png)
			- ![image.png](../assets/image_1654938823719_0.png)
			- ![image.png](../assets/image_1654938886359_0.png)
			- ![image.png](../assets/image_1654938952165_0.png)
			-
		- [[Weak fairness]]
		  collapsed:: true
			- Enabled
			  collapsed:: true
				- ![image.png](../assets/image_1654939854964_0.png)
				- ![image.png](../assets/image_1654939886841_0.png)
				- ![image.png](../assets/image_1654939953427_0.png)
			- ![image.png](../assets/image_1654940198438_0.png)
			- ![image.png](../assets/image_1654940295397_0.png)
			- ![image.png](../assets/image_1654940335495_0.png)
				-
				-
		- Adding Liveness To a Spec
		  collapsed:: true
			- ![image.png](../assets/image_1654940535914_0.png)
			- ![image.png](../assets/image_1654940629627_0.png)
			- ![image.png](../assets/image_1654940719093_0.png)
			- ![image.png](../assets/image_1654940736620_0.png)
			- ![image.png](../assets/image_1654940749061_0.png)
			- ![image.png](../assets/image_1654940997075_0.png)
			- ![image.png](../assets/image_1654941005406_0.png)
			- ![image.png](../assets/image_1654941347752_0.png)
			- ![image.png](../assets/image_1654941364048_0.png)
			- ![image.png](../assets/image_1654941420303_0.png)
			-
	- The Protocol
		- The Safety Specification
		  collapsed:: true
			- ![image.png](../assets/image_1654947591936_0.png)
			- ![image.png](../assets/image_1654947766815_0.png)
			-
		- Liveness
		  collapsed:: true
			- ![image.png](../assets/image_1654947876968_0.png)
			- ![image.png](../assets/image_1654947902290_0.png)
			- ![image.png](../assets/image_1654947953096_0.png)
			- ![image.png](../assets/image_1654948112931_0.png)
			- ![image.png](../assets/image_1654948228014_0.png)
			- ![image.png](../assets/image_1654948358026_0.png)
			- ![image.png](../assets/image_1654948375855_0.png)
			- ![image.png](../assets/image_1654948444470_0.png)
				- ![image.png](../assets/image_1654948481078_0.png)
				- ![image.png](../assets/image_1654948514105_0.png)
				- ![image.png](../assets/image_1654948570546_0.png)
			-
- Implement With Refinement
	- Preliminaries
		- Recursive Definitions
		  collapsed:: true
			- ![image.png](../assets/image_1654951801624_0.png)
			- ![image.png](../assets/image_1654951837196_0.png)
			- ![image.png](../assets/image_1654951948491_0.png)
			- ![image.png](../assets/image_1654952006461_0.png)
		- Simple Substitution Law
		  collapsed:: true
			- ![image.png](../assets/image_1654952046779_0.png)
			- ![image.png](../assets/image_1654952190377_0.png)
			- ![image.png](../assets/image_1654952229783_0.png)
			- ![image.png](../assets/image_1654952290939_0.png)
			- ![image.png](../assets/image_1654952388780_0.png)
			-
		- The Temporal Substitution law
		  collapsed:: true
			- ![image.png](../assets/image_1654952986901_0.png)
			- ![image.png](../assets/image_1654953043857_0.png)
			- ![image.png](../assets/image_1654953203935_0.png)
		- The General Temporal Substitution Law
		  collapsed:: true
			- ![image.png](../assets/image_1654954266964_0.png)
		- The AB2 Protocol
		  collapsed:: true
			- ![image.png](../assets/image_1655016959966_0.png)
			- ![image.png](../assets/image_1655017705132_0.png)
			- ![image.png](../assets/image_1655017780502_0.png)
			- ![image.png](../assets/image_1655017828312_0.png)
			- Model Value
				- TLC assumes a model value does not equal any value that you might expect it to be different from.
			- Liveness Of AB2
				- ![image.png](../assets/image_1655019697190_0.png)
				- ![image.png](../assets/image_1655019877049_0.png)
				- ![image.png](../assets/image_1655019922585_0.png)
				- ![image.png](../assets/image_1655019942949_0.png)
				- ![image.png](../assets/image_1655020006207_0.png)
	- Refinement Mappings
		- AB2 Implements AB
		  collapsed:: true
			- ![image.png](../assets/image_1655022870328_0.png)
			- ![image.png](../assets/image_1655022901309_0.png)
			- ![image.png](../assets/image_1655023052370_0.png)
			- ![image.png](../assets/image_1655024502948_0.png)
				- What does it mean
				  collapsed:: true
					- ![image.png](../assets/image_1655024575634_0.png)
					- ![image.png](../assets/image_1655024626478_0.png)
					- ![image.png](../assets/image_1655024665741_0.png)
					- ![image.png](../assets/image_1655024718394_0.png)
					- ![image.png](../assets/image_1655024741857_0.png)
				- How do we check it
				  collapsed:: true
					- ![image.png](../assets/image_1655024769315_0.png)
					- Specifying SpecH
						- ![image.png](../assets/image_1655024891017_0.png)
						- ![image.png](../assets/image_1655024931900_0.png)
						- ![image.png](../assets/image_1655025061713_0.png)
						- ![image.png](../assets/image_1655025091831_0.png)
						- ![image.png](../assets/image_1655025154766_0.png)
						- ![image.png](../assets/image_1655025175251_0.png)
						- ![image.png](../assets/image_1655025200137_0.png)
					- Checking Implementation
						- ![image.png](../assets/image_1655025513092_0.png)
						- ![image.png](../assets/image_1655025561380_0.png)
						- ![image.png](../assets/image_1655025636130_0.png)
						- ![image.png](../assets/image_1655025709890_0.png)
					- Simplifying Refinement
						- ![image.png](../assets/image_1655025853562_0.png){:height 307, :width 572}
						- ![image.png](../assets/image_1655025908175_0.png)
						- ![image.png](../assets/image_1655026168689_0.png)
						- ![image.png](../assets/image_1655026201249_0.png)
						- ![image.png](../assets/image_1655026238014_0.png)
						- ![image.png](../assets/image_1655026374316_0.png)
						- ![image.png](../assets/image_1655026422718_0.png)
						- ![image.png](../assets/image_1655026435713_0.png)
						- ![image.png](../assets/image_1655026449754_0.png)
						- ![image.png](../assets/image_1655026464585_0.png)
						- ![image.png](../assets/image_1655026521578_0.png)
						- ![image.png](../assets/image_1655042474060_0.png)
						- ![image.png](../assets/image_1655042514367_0.png)
						- ![image.png](../assets/image_1655042571878_0.png)
						- ![image.png](../assets/image_1655042605155_0.png)
						- ![image.png](../assets/image_1655042661943_0.png)
						- ![image.png](../assets/image_1655042689129_0.png)
						- ![image.png](../assets/image_1655042712419_0.png)
						- ![image.png](../assets/image_1655042725719_0.png)
						- ![image.png](../assets/image_1655042747839_0.png)
						- ![image.png](../assets/image_1655042775584_0.png)
				- What we did and why
				  collapsed:: true
					- ![image.png](../assets/image_1655046946040_0.png)
					- ![image.png](../assets/image_1655047314329_0.png)
					- ![image.png](../assets/image_1655047336203_0.png)
					- ![image.png](../assets/image_1655047368960_0.png)
					- ![image.png](../assets/image_1655047435841_0.png)
					- ![image.png](../assets/image_1655047461306_0.png)
					- ![image.png](../assets/image_1655047497230_0.png)
					- ![image.png](../assets/image_1655047520433_0.png)
					- ![image.png](../assets/image_1655047555968_0.png)
					-
				-
		- Imaginary variables
		  collapsed:: true
			- ![image.png](../assets/image_1655047672564_0.png)
			- ![image.png](../assets/image_1655047691672_0.png)
			- ![image.png](../assets/image_1655047713021_0.png)
			- ![image.png](../assets/image_1655047734754_0.png)
			- ![image.png](../assets/image_1655047753467_0.png)
			- ![image.png](../assets/image_1655047781350_0.png)
			- ![image.png](../assets/image_1655047825131_0.png)
			- ![image.png](../assets/image_1655047846474_0.png)
			- ![image.png](../assets/image_1655047926826_0.png)
			- ![image.png](../assets/image_1655047951806_0.png)
			- ![image.png](../assets/image_1655047970471_0.png)
			-
		-
		-