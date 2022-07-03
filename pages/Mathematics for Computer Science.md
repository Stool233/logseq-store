- Video Course
	- 6.042J
		- This subject offers an introduction to discrete mathematics oriented toward computer science and engineering.
	- Quick Summary
		- Fundamental Concepts of Discrete Mathematics (sets, relations, proof methods,… )
			- 离散数学基本概念(集合、关系、证明方法……)
		- Discrete Mathematical Structures (numbers, graphs, trees, counting…)
			- 离散数学结构(数字、图、树、计数……)
		- Discrete Probability Theory
			- 离散型概率理论
	- Proofs
		- A Cool Proof
			- power point
			  collapsed:: true
				- ![image.png](../assets/image_1656307254777_0.png)
					- ![image.png](../assets/image_1656307265888_0.png)
					-
			- elegant and correct
				- --in this case
			- worrisome in general
				- --hidden assumptions
		- Bogus Proof
			- power point
			  collapsed:: true
				- ![image.png](../assets/image_1656307333565_0.png)
				- ![image.png](../assets/image_1656567327682_0.png)
				- ![image.png](../assets/image_1656567425908_0.png)
				-
			- Moral
				- Be sure rules are properly applied.
				- Thoughtless calculation no  substitute for understanding.
-
- Teaching Materials
	- What is a Proof
		- proposition（命题）
			- A proposition is a statement (communication) that is either true or
			  false.
		- Predicates（谓词）
			- A predicate can be understood as a proposition whose truth depends on the value
			  of one or more variables.
			- Predicate and function
				- This notation for predicates is confusingly similar to ordinary function notation.
				  If P is a predicate, then P(n) is either true or false, depending on the value of n.
				  On the other hand, if p is an ordinary function, like n2+1, then p(n) is a numerical
				  quantity. Don’t confuse these two!
		- The Axiomatic Method ( 公理化方法 )
			- axioms（公理）
				- Propositions like these that are simply accepted as true are called
				  axioms
			- proof（证明）
				- A proof is a sequence of logical deductions from axioms and previously proved statements that concludes with the proposition in question.
				- 证明 是指从公理及已被证明的语句，推导出命题结论的一系列逻辑推理过程。
			- There are several common terms for a proposition that has been proved. The different terms hint at the role of the proposition within a larger body of work
				- Important true propositions are called theorems（定理）
				- A lemma（引理） is a preliminary proposition useful for proving later propositions.
				- A corollary（推论） is a proposition that follows in just a few logical steps from a
				  theorem.
			- Euclid’s axiom-and-proof approach（欧几里得的公理-证明方法）, now called the axiomatic method（公理化方法）, remains the foundation for mathematics today.
				- In fact, just a handful of axioms, called the Zermelo-Fraenkel with Choice axioms(ZFC), together with a few logical deduction rules, appear to be sufficient to derive essentially all of mathematics.
		- Our Axioms（我们的公理）
			- So instead of starting with ZFC, we’re going to take a huge set of axioms as our foundation: we’ll accept all familiar facts from high school math.
				- This will give us a quick launch, but you may find this imprecise specification
				  of the axioms troubling at times.
				- For example, in the midst of a proof, you may start to wonder, “Must I prove this little fact or can I take it as an axiom?”
					- There really is no absolute answer, since what’s reasonable to assume and what requires proof depends on the circumstances and the audience.
					- A good general guideline is simply to be up front about what you’re assuming.
			- Logical Deductions（逻辑推理）
				- Logical deductions, or inference rules, are used to prove new propositions using
				  previously proved ones.
					- 逻辑推理，或推理规则，是指基于已被证明过的命题来证明新的命题。
				- A fundamental inference rule is modus ponens（假言推理）. This rule says that a proof of P together with a proof that P IMPLIES Q is a proof of Q.
					- 一个基本的推理规则是假言推理，即证明了P并且证明了P IMPLIES Q
		-