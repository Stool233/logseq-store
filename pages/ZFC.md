## **策梅洛-弗兰克尔集合论（ZFC）**
	- 策梅洛-弗兰克尔[[集合论]]（ZFC）是数学中使用最广泛的集合论公理系统。它由两部分名称组成：策梅洛（Ernst Zermelo）和弗兰克尔（Abraham Fraenkel），两位数学家对早期集合论公理进行了显著的发展和改进。
- ### 历史背景
	- 集合论的发展初期，由Georg Cantor在19世纪末提出，但早期集合论面临一些悖论，最著名的是罗素悖论。为解决这些问题，Zermelo在1908年提出了一套集合论公理，后来Fraenkel和Thoralf Skolem对其进行了改进和扩展，形成了现在所称的ZFC。
- ### 公理系统
	- ZFC包括以下几个基本公理（或公理模式）：
		- 1. **[[外延公理]]（Axiom of Extensionality）**
			- 两个集合相等当且仅当它们含有相同的元素。
			- $$
			  \forall A \forall B (\forall x (x \in A \leftrightarrow x \in B) \rightarrow A = B)
			  $$
		- 2. **[[空集公理]]（Axiom of Empty Set）**
			- 存在一个没有任何元素的集合，称为空集，通常表示为∅。
			- $$
			  \exists B \forall x (x \notin B)
			  $$
		- 3. **配对公理（Axiom of Pairing）**
			- 对任何两个集合A和B，存在一个集合，这个集合的元素正是A和B。
			- $$
			  \forall A \forall B \exists C \forall x (x \in C \leftrightarrow (x = A \lor x = B))
			  $$
		- 4. **并集公理（Axiom of Union）**
			- 对于任何一个集合的集合，存在一个集合，包含所有原集合的元素。
			- $$
			  \forall A \exists B \forall x (x \in B \leftrightarrow \exists C (x \in C \land C \in A))
			  $$
		- 5. **幂集公理（Axiom of Power Set）**
			- 对于任何集合A，存在一个集合，包含A的所有子集。
			- $$
			  \forall A \exists B \forall x (x \in B \leftrightarrow x \subseteq A)
			  $$
		- 6. **替代公理模式（Axiom Schema of Replacement）**
			- 如果一个函数性质能将一个集合的每个元素映射到一个集合，则存在一个集合，包含所有映射的结果。
			- $$
			  \forall A (\forall x \in A \exists ! y \phi(x, y) \rightarrow \exists B \forall y (y \in B \leftrightarrow \exists x \in A \phi(x, y)))
			  $$
		- 7. **无穷公理（Axiom of Infinity）**
			- 存在一个集合，包含空集，并且对每个元素x，该集合也包含x与{x}的并集。
			- $$
			  \exists A (\emptyset \in A \land \forall x (x \in A \rightarrow x \cup \{x\} \in A))
			  $$
		- 8. **正则公理（Axiom of Regularity）**
			- 每个非空集合x都包含一个成员y，使得x和y不相交。
			- $$
			  \forall A (A \neq \emptyset \rightarrow \exists x (x \in A \land x \cap A = \emptyset))
			  $$
		- 9. **[[选择公理]]（Axiom of Choice）**
			- 对于任何集合的集合，如果它们的每个成员都非空且相互不交，那么存在一个集合，它从每一个成员集合中各选一个元素。
			- 更形式化地说：设 \(\mathcal{C}\) 是一族非空集合。选择公理表明存在一个函数 \(f\)（称为选择函数），使得对于所有 \(A \in \mathcal{C}\)，有 \(f(A) \in A\)。即：
			- $$
			  \forall \mathcal{C} (\emptyset \notin \mathcal{C} \rightarrow \exists f (\forall A \in \mathcal{C} (f(A) \in A))) 
			  $$
			-
-
-
- ### 重要性和应用
	- ZFC是现代数学的基石之一，它提供了一个坚实的基础，使数学家可以在不担心悖论的情况下探索无限和极限。此外，ZFC在逻辑、计算机科学、语义学等领域有广泛的应用。
	- ZFC并非没有争议，有些数学家和哲学家探讨了它的完备性、一致性和选择公理的必要性。尽管存在这些讨论，ZFC仍然是大多数数学研究中使用的标准集合论框架。
-
-