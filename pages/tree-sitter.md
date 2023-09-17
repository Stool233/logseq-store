- tree-sitter-graph
	- https://docs.rs/tree-sitter-graph/0.11.0/tree_sitter_graph/reference/index.html
	- tree-sitter-graph的图定义DSL
		- High-level structure
			- 图DSL由一个或多个节(stanzas)组成
				- 每个节以tree-sitter查询模式[***query pattern***](https://tree-sitter.github.io/tree-sitter/using-parsers#pattern-matching-with-queries),开始
					- 该模式标识具体语法树的一部分
					- 查询模式应该使用捕获[***captures***](https://tree-sitter.github.io/tree-sitter/using-parsers#capturing-nodes)来识别树的匹配部分中的特定语法节点
				- 查询之后是一个块，它是一个语句序列，用于构造图节点（node），将它们与边（edge）连接起来，并用属性（attributes）对两者进行注释.
			- 为了针对具体的语法树执行图形DSL文件，我们详尽地执行图形DSL文件中的每个节。
				- 对于每个节，我们标识具体语法树与查询模式匹配的每个位置。
					- 对于这些位置中的每一个，我们最终都为查询模式的捕获分配了一组不同的语法节点。
					- 我们为每个捕获赋值执行语句块，创建块中提到的任何图节点、边或属性。
				- 常规执行将按顺序应用节，重要的是要确保在使用有作用域的变量之前已经赋值。
					- 在使用延迟求值策略(隐式处理此问题)时，这不是必需的。
						- 当有许多节时，惰性求值策略也更有效，因为它可以减少树遍历。
						- 因此，建议使用延迟求值策略，并且很可能成为未来版本中唯一支持的策略。
		- Expressions
			- The value of an expression in the graph DSL can be any of the following:
				- - null
				- - a boolean
				- - a string
				- - an integer (unsigned, 32 bits)
				- - a reference to a syntax node
				- - a reference to a graph node
				- - an ordered list of values
				- - a list comprehension
				- - an unordered set of values
				- - a set comprehension
		- Syntax nodes（语法节点）
			- 语法节点由tree-siter查询捕获(@name)标识
			- 未使用的查询捕获被视为错误，除非它们以下划线开头。
		- Variables（变量）
			- 可以使用变量在图形DSL文件中的不同节和语句之间传递信息。
			- 变量有三种:
				- 全局变量（***Global*** variables）由执行图形DSL文件的任何进程在外部提供。
					- 例如，您可以使用它来传入正在分析的源文件的路径，以作为您创建的图结构的一部分使用。
				- 局部变量（***Local*** variables）仅在当前节的当前执行中可见。
					- 一旦节中的所有语句执行完毕，所有局部变量就会消失。
				- 范围变量（***Scoped*** variables）是“附加到”语法节点上的。
					- 它们的值会从一个节传递到下一个节。范围变量通过使用语法节点表达式（通常是查询捕获）和变量名引用，二者之间用一个点分隔：@node.variable。
		- Functions（函数）
			- 执行图形 DSL 文件的进程可以提供在图形 DSL 段落中调用的函数。
			- 函数调用使用类似 Lisp 的语法，被调用的函数名位于括号内。函数调用的参数是任意表达式。
			- 请注意，决定哪些函数可用的是执行图形 DSL 文件的进程。虽然我们确实定义了一个标准库，并且大多数时候这些就是可用的函数，但你应该仔细检查你正在使用的图形 DSL 工具的文档以确保正确。
		- 图节点（Graph nodes）
			- 你可以使用这个图形 DSL 来创建你想要的任何图形结构。你创建哪些图节点，以及如何使用边来连接图节点都没有限制。你不仅限于创建一棵树，特别是你不限于创建一棵与解析语法树“对齐”的树。
			- 创建新的图节点有两种方式。
				- 第一种，也是最常见的方式，是使用 `node` 语句：
					- ```
					  (identifier) @id
					  {
					    node @id.node
					  }
					  ```
				- 创建图节点的第二种方式是调用 `node` 函数
					- ```
					  (identifier) @id
					  {
					    let @id.node = (node)
					  }
					  ```
				- 注意，这两个例子做的事情完全相同！
					- 实际上，第一个例子中的 `node` 语句只是第二个例子中 `let` 语句的语法糖。
					- 由于最常见的模式是创建一个图节点并立即将其赋值给一个不可变的作用域变量，我们提供了 `node` 语句作为一个方便的简写。
					- 如果你需要做更复杂的事情，比如将图节点的引用赋值给一个可变变量，你可以直接调用 `node` 函数。
			- 通过使用作用域变量[scoped variable](https://docs.rs/tree-sitter-graph/0.11.0/tree_sitter_graph/reference/index.html#variables)将图节点附加到语法节点，你可以从多个节（stanza）引用它们：
				- ```
				  (identifier) @id
				  {
				    node @id.node
				  }
				  
				  (dotted_name (identifier) @dotted_element)
				  {
				    ; We will learn more about the attr statement below
				    attr (@dotted_element.node) kind = "dotted"
				    }
				  ```
				- 在这个例子中，我们会为每个`identifier`语法节点创建一个图节点。
					- 每个语法节点都会有一个节点作用域变量，包含对其图节点的引用。
					- 但只有那些出现在 `dotted_name` 内部的标识符的图节点会有一个 `kind` 属性。
					- 尽管我们在不同的查询中使用了不同的捕获名称来查找这些`identifier`节点，但两个节中的图节点引用都指向同一个图节点。
		- 图边（Edges）
			- 边通过 `edge` 语句创建，该语句指定应连接的两个图节点。边是有方向的，`edge` 语句中的 `->` 箭头表示边的方向
				- ```
				  (import_statement name: (_) @name)
				  {
				    node @name.source
				    node @name.sink
				    edge @name.source -> @name.sink
				  }
				  ```
			- 在图中，任何特定的源节点和目标节点之间最多只能有一条边。如果多个节（stanza）在同一对图节点之间创建边，那么这些边将被 "折叠" 成一条边。
		- 属性（Attributes）
			- 图节点和边都有一组关联的属性。每个属性有一个名字（是一个标识符）和一个值。
			- 你可以使用 `attr` 语句来给图节点或边添加属性：
				- ```
				  (import_statement name: (_) @name)
				  {
				    node @name.source
				    node @name.sink
				    attr (@name.sink) kind = "module"
				    attr (@name.source -> @name.sink) precedence = 10
				  }
				  ```
			- 注意，你必须已经创建了图节点或边，并且该图节点或边不能已经有同名的属性。
			- 属性可能看起来类似于作用域变量，但它们其实很不同。
				- 属性被附加到图节点和边上，而作用域变量被附加到语法节点上。
				- 更重要的是，作用域变量只在执行图描述语言（DSL）文件时存在。一旦执行完成，变量就会消失。
				- 另一方面，属性是图 DSL 文件产生的输出的一部分，执行完成后仍然存在。
		- 属性简写（Attribute shorthands）
			- 常用的属性组合可以用简写方式来表示。每个简写定义了属性名，一个捕获属性值的变量，以及它扩展的一系列属性。
			- 属性简写在与节（stanzas）相同的级别上定义。例如，以下简写接受一个语法节点作为参数，并扩展为其源文本和子索引的属性：
				- ```
				  attribute node_props = node => node_text = (source-text node), node_index = (named-child-index node)
				  
				  (argument (_)@expr) {
				    attr (@expr) node_props = @expr
				  }
				  ```
		- 正则表达式
			- 你可以使用 `scan` 语句来将字符串值与一组正则表达式进行匹配。
		- 条件语句（Conditionals）
			- 你可以使用 `if` 语句使语句块根据可选值进行条件处理。条件是由逗号分隔的语句列表。`some EXPRESSION` 子句表示可选值必须存在。 `none EXPRESSION` 子句表示可选值缺失。裸表达式会被计算为布尔值。条件中的所有值必须是本地的，这意味着它们不能从作用域变量派生。
		- 列表迭代（List iteration）
			- 你可以使用 `for` 语句来对列表中的每个元素执行一组语句。列表值必须是本地的，这意味着它不能从作用域变量派生。
		- 调试（Debugging）
			- 为了支持 "古老和和谐的Printf调试器秩序" 的成员，你可以使用 `print` 语句在执行图形DSL文件期间打印出任何表达式的内容（到stderr）：
				- ```
				  (identifier) @id
				  {
				     let x = 4
				     print "Hi! x = ", x
				  }
				  ```
		-