- 来源：
	- https://about.sourcegraph.com/blog/announcing-scip
-
- 扩大[[LSIF]]规模的挑战
	- 随着 LSIF 使用量的增长，我们遇到了该协议的一些限制：
		- 由于缺乏静态类型而导致开发速度缓慢。
			- LSIF 不附带机器可读的模式，并且动态图结构使得在大多数编程语言中很难将 LSIF 有效负载编码为一组简单的结构或类。
		- 性能缓慢导致在写入或读取 LSIF 有效负载的图形编码时需要保存大量内存数据结构。
		- 由于大量使用不透明的 ID 号对图结构进行编码，导致手动调试原始 LSIF 有效负载变得困难。
		- 实现增量索引的复杂性，这对于大型代码库来说是必要的。
			- 不透明全局 ID 的大量使用对符号（或“结果集”）添加到索引的方式施加了排序约束，使得处理文件中的循环依赖关系以及其他常见情况变得棘手。
			- 全局递增的 ID 也使得仅使用文档子集的新信息更新现有索引变得困难。
	- 这些问题大部分都归结为 LSIF 的图编码，它严重依赖不透明的 ID 号来连接边和顶点。为了解决这些问题，我们创建了 SCIP 作为 Protobuf 模式，该模式以人类可读的符号字符串 ID 为中心，取代了“名字对象（monikers）”和“结果集（resultSet）”的概念。
	-
- [[SCIP]]：更快、更小、更简单的语言索引器
	- SCIP Protobuf 模式可[在 sourcegraph/scip](https://sourcegraph.com/github.com/sourcegraph/scip/-/blob/scip.proto)存储库中找到，并包含有关如何对符号和源位置之间的关系进行编码的综合文档。
		- SCIP 的设计深受[SemanticDB](https://scalameta.org/docs/semanticdb/specification.html)的启发
			- SemanticDB 是 Scala 生态系统中首创的另一种代码索引格式。
	- 开始使用 SCIP 后，我们体验到了一系列好处
		- 与 LSIF 相比，使用 SCIP 时生活质量得到了多项改进，因此实现新语言索引器的开发时间更快。
			- 这些改进包括使用 Protobuf 架构中的静态类型，在编辑器中为我们提供丰富的代码补全功能，并降低因拼写错误导致的运行时错误的风险。
			- 由于以人类可读的符号而不是不透明的数字 ID 为中心，我们还经历了更符合人体工程学的调试，并且减少了记录不必要的抽象的需要，例如导入/导出名字，如果出错，这些名字会默默地破坏导航。
		- 索引器的运行性能更好。例如，当我们用scip-typescript替换lsif-node时，我们在CI中体验到了10倍的速度提升（尽管注意这个速度提升并不仅仅归因于协议差异）。
		- 生成的索引文件占用更少的磁盘空间。我们观察到，与同等 SCIP 有效负载相比，gzip 压缩后的 LSIF 索引平均大 4 倍。未压缩的 LSIF 有效负载大约大 5 倍。
		- 索引器更容易测试。我们在 SCIP 之上构建了一个快照测试实用程序，我们可以跨索引器重用该实用程序。相比之下，根据我们的经验，使用 LSIF 有效负载进行快照测试是很[痛苦的。](https://github.com/sourcegraph/scip/pull/27/files#diff-9c76847e0d19bedf4d9afbdfbe5e11046b73d78c80437d6adf7c6e7704052c66R23)
	- 展望未来，我们预计 SCIP 还将解锁我们之前难以通过 LSIF 支持的以下用例：
		- **增量索引**：一旦实现，SCIP 用户在 git 推送后将在 Sourcegraph 上获得精确代码导航的等待时间更短，因为我们的后端只需要索引已更改的文件，而不是每次提交时的整个存储库。
		- **跨语言导航**：例如，一旦实现，SCIP 用户将能够在 Protobuf 和生成的 Java/Go Protobuf 绑定之间导航，帮助他们找到以前无法通过基于搜索和精确代码导航的相关代码示例。
-
- 一些个人记录
	- 看了一下，对于各种语言的支持方式，更多的还是基于编译器的能力去做一些解析，然后建立索引。
	- 在目前的时间节点（2023-09-24），暂时不支持增量建立索引
	-
		-
		-