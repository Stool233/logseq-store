- Tags: [[TLA+]]
- Fairness, Weak and Strong
	- There are two kinds of fairness: weak and strong.（公平性有两种：弱公平性和强公平性。）
		- A weakly fair action will, if it stays enabled, eventually happen.（如果一个动作保持可执行，那么弱公平性将最终发生）
			- We can declare every label in a process weakly fair by calling it a fair process.（我们可以通过称其为公平过程，来声明流程中的每个标签都具有弱公平性）
		- A strongly fair action, if it’s repeatedly enabled, will eventually happen.（如果一个动作反复地变为可执行，那么强公平性将最终发生。）
			- There can be gaps in between, but as long as there’s some cycle where it keeps getting enabled again, it will happen.（之间可能会有间隙，但只要存在某个循环使其不断再次变为可执行，那么它就会发生。）
			- In practice, people don’t often use strong fairness; it’s a much safer to assume the system is only weakly fair.（在实践中，人们往往不常使用强公平性；假设系统只具有弱公平性是更安全的。）
				- However, it’s worth knowing about for the cases where it is useful.（然而，对于它有用的情况，了解强公平性是值得的。）
-
- The Temporal Operators
	- []
		- [] is always.
		- []P means that for P is true for all states in all behaviors.
			- []P表示对于所有行为的所有状态P都成立。
		- You can also write ~[]P, which means that P will be false for at least one state.
			- 你也可以写~[]P，这意味着P至少在一个状态下为假。
	- <>
		- <> is eventually.
		- <>P means that for every behavior, there is at least one state where P is true.（<>P表示对于每个行为，至少有一个状态P为真。）
			- It may be false before, and it may be false after, but what matters is that it was at some point true. （它之前可能是假的，之后也可能是假的，但重要的是它在某个时刻是真的。）
		- You can also write ~<>P, which means that P is never true.（你也可以写~<>P，这意味着P永远不为真。）
			- Note that this is the same as saying []~P, and in fact <>P is formally defined as ~[]~P. （注意，这与说[]~P是一样的，事实上<>P被正式定义为~[]~P。）
	- ~>
		- ~> is leads-to. P ~> Q means that if there is some state where P is true, then either Q is true either now or in some future state.（P ~> Q意思是，如果有某个状态P为真，那么Q要么是现在为真，要么是未来为真）
		- Once this is set, it’s irreversible: even if P is later false, Q still must happen.（一旦这个被设定，它是不可逆的:即使P后来为假，Q仍然必须发生。）
			- Unlike <>, ~> is “triggered” every time P is true.（与 <> 不同，每当 P 为真时，~> 将被“触发”。）
		- You can also do P ~> []Q. If P is true, then there is some state where Q becomes true and forever stays true.（你也可以使用 P ~> []Q。如果 P 为真，那么就存在某个状态使 Q 变为真，并且永远保持真。）
	- [ ]<> and <>[ ]
		- []<>P means that P is always eventually true, <>[]P means that P is eventually always true.（[]<>P 表示 P 最终总是为真，<>[]P 表示 P 最后始终为真。）
			- For a finite spec, these mean the same thing: P is true at termination.（对于有限的规范，这些意味着同样的事情：P 在终止时为真。）
			- For an infinite spec,（对于无限的规范）
				- <>[]P means that there is some point where P becomes true and forever stays true（<>[]P 表示存在某个点，P 变为真并且永远保持为真）
				- []<>P means that if P ever becomes false, it will eventually become true again.（[]<>P 表示，如果 P 曾经变为假，它最终将再次变为真）
					- Another way to think about it is that []<>P <=> (~P ~> P): P being false leads to P being true later.（另一种理解方式是，[]<>P <=> (~P ~> P)：P 的假导致 P 在后来变为真。）
		- 这两个表达式的主要区别在于它们的持续性和反复性。
			- `[]<>P` 允许 `P` 反复在真和假之间变化，只要它最终总是变回真即可；
			- 而 `<>[]P` 要求 `P` 在某个时刻之后保持真，不能再变为假。
-
- Fairness in TLA+（TLA+中的公平性）
	- 两个关键字：
		- `ENABLED A` 表示如果在当前步骤中，动作A可能成为下一个步骤即它能够被执行，那么这个表达式为真。
		- `<<A>>_v` 表示动作A发生且变量v发生变化。与之相对的是`[A]_v`，表示动作A发生或变量v不变。
	- TLA+中公平性的形式化定义如下：
		- `WF_v(A)` 定义为 `<>[](ENABLED <<A>>_v) => []<><<A>>_v`
		- `SF_v(A)` 定义为 `[]<>(ENABLED <<A>>_v) => []<><<A>>_v`
	- 解释这两个公式：
		- `WF_v(A)`（A 是弱公平的）：如果最终总是能够发生A动作（以改变变量v的方式），那么最终这个动作将会发生（并改变变量v）。
		- `SF_v(A)`（A 是强公平的）：如果总是最终能够发生A动作（以改变变量v的方式），那么最终这个动作将会发生（并改变变量v）。
	- 公平性约束被添加到规范的定义中。可以在之前的强公平性例子的转换中看到这一点：
		- ```tla
		  Spec == /\ Init
		        /\ [][Next]_vars
		        /\ \A self \in Threads : SF_vars(thread(self))
		  ```
		- 这里的 `Spec` 定义了什么算作一个有效的迹（trace）。公平性是一个额外的约束，它排除了一些情况，比如无限的停顿（stutters）。
	- 注意，通过写 `\A self \in Threads : SF_vars(thread(self))`，我们实质上是在对每个线程都施加公平性约束。
		- 如果我们写 `\E`，那么我们就是在说至少一个线程是公平的，但其他线程可能是不公平的。
	- 如果这两个条件对你来说都是直观的，那么我会说你已经完全理解了纯粹的TLA+是如何工作的。
	-
-
- `InvalidationMessage == [key: KEYS]`
	- 定义了一个名为 `InvalidationMessage` 的运算符，它表示一个具有单一字段 `key` 的记录（类似于其他编程语言中的对象或字典），`key` 的取值范围为 `KEYS`。
	- 这个定义的含义取决于 `KEYS` 的定义，`KEYS` 可以是任何集合，例如，如果 `KEYS` 被定义为 `{1, 2, 3}`，那么 `InvalidationMessage` 就表示包含所有 `key` 字段值为 `{1, 2, 3}` 中某个元素的记录的集合。
		- 以下是一些可能的 `InvalidationMessage` 的值的示例：
			- 如果 `KEYS` = `{1, 2, 3}`，那么 `InvalidationMessage` 可能表示为 `[key |-> 1]`，`[key |-> 2]` 或 `[key |-> 3]`。
			- 如果 `KEYS` 是集合 `{a, b, c}`，那么 `InvalidationMessage` 可能表示为 `[key |-> a]`，`[key |-> b]` 或 `[key |-> c]`。
	- 在 TLA+ 中，你可以使用 `:` 来定义一个记录的可取值范围。
		- 例如，`[x : S]` 表示一个字段为 `x` 的记录，其中 `x` 的值可以是集合 `S` 中的任何元素。
	- 需要注意的是，`InvalidationMessage` 是一个集合，它包含的每个元素都是一个记录，这个记录有一个 `key` 字段，其值可以是集合 `KEYS` 中的任何元素。
-
- `[key |-> 1]`
	- 在 TLA+ 中，`[key |-> 1]` 表示一个记录（类似于其他编程语言中的对象或字典），该记录具有一个字段 `key`，其值为 `1`。
		- 在 TLA+ 中，`|->` 符号用于记录的定义。
			- 在这种情况下，`key |-> 1` 定义了一个记录，其中 `key` 是字段名，`1` 是这个字段的值。
		- 以下是一些其他的例子：
			- - `[name |-> "Alice", age |-> 25]` 表示一个有两个字段的记录：字段 `name` 的值为 `"Alice"`，字段 `age` 的值为 `25`。
			- - `[x |-> 2, y |-> 3]` 表示一个有两个字段的记录：字段 `x` 的值为 `2`，字段 `y` 的值为 `3`。
		- 在 TLA+ 中，你可以使用这种方式定义任意数量的字段，以创建复杂的记录结构。
-
- `[KEYS -> DataVersion]`
	- 在TLA+中，`[KEYS -> DataVersion]` 表示一个函数，该函数将集合 `KEYS` 中的每个元素映射到 `DataVersion`。
	- 在这种情况下，`->` 符号用于定义函数。函数的左边是它的域（在这里是 `KEYS` 集合），右边是它的值空间（在这里是 `DataVersion`）。
-
-
-
-
- [[MapReduce]]
	- 三种不同假设下的形式描述
		- 1. A first spec that assumes all workers always succeed.（假设所有的worker都能成功。）
		- id:: 6382c9cc-e87d-447e-96d1-46835002201c
		  2. A second, fault tolerant spec that allows workers to fail.（允许worker出错的容错规范。）
			- The reducer is fair. If it’s not, we can’t guarantee anything happens.
			- There is at least one fair worker. If there’s none, then we can easily see the algorithm couldn’t possible succeed: just have every worker keep crashing and you’ll never meet Liveness.
			- It doesn’t matter which worker is the fair one. This assumption significantly reduces our state space, since we can arbitrarily pick one with CHOOSE.
			- The reducer may or may not detect an unfair worker failing, but it will never falsely decide a fair worker has failed. This is the biggest assumption here, but it’s an assumption that makes our system a lot easier to design.
		- 3. A final spec that works even if the recovery mechanism partially fails, too.（即使恢复机制部分失败也能工作）
			- In theory, we don’t have a way of distinguishing failing nodes from passing ones. In practice, we can do things that give us a reasonable amount of confidence.
				- For example, we can ping all the servers every N seconds and assume that the ones that don’t answer in time are failing. Of course, the node might not be failing, and it could be that our reducer is acting up.
			- Before, the server could move the queue of an unfair worker to a fair one. Now, the server can still move the assignments of an unfair worker but does not know which ones are fair. Instead, it must decide which worker to pick.
				- We will continue to assume the system never reassigns away from a fair worker, as that worker always responds to the heartbeat.
				-
- While blind guessing can work for tests or typecheckers, it won’t help with specification.
-
-
- PlusCal
	- PlusCal is a language for writing algorithms. It is designed not to replace programming languages, but to replace pseudo-code.
		- Why replace pseudocode? No formal language can be as powerful or easy to write. Nothing can beat the convenience of inventing new constructs as needed and letting the reader try to deduce their meaning from informal explanations.
	- The major problem with pseudo-code is that it cannot be tested, and untested code is usually incorrect.
		- In August of 2004, I did a Google search for quick sort and tested the first ten actual algorithms on the pages it found. Of those ten, four were written in pseudo-code; they were all incorrect. The only correct versions were written in executable code; they were undoubtedly correct only because they had been debugged.
	- Algorithms written in PlusCal can be tested with TLC—either by complete model checking or by repeated execution, making nondeterministic choices randomly
	- Another advantage of an algorithm written in PlusCal is that it has a precise meaning that is specified by its TLA+ translation.
		- The translation can be a practical aid to understanding the meaning of the code.
		- Since the translation is a formula of TLA, a logic with well-defined semantics and proof rules , it can be used to reason about the algorithm with any desired degree of rigor
	- PlusCal is a language with simple program structures and arbitrary mathematical expressions.
	- PlusCal is meant to replace pseudo-code. It combines the best features of pseudo-code with the ability to catch errors by model checking.
		- It is suitable for use in books, in articles, and in the classroom. It can also be used by programmers to debug their algorithms before implementing them.
-
-