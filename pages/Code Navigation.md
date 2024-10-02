- Code Navigation（代码导航）相关的一些智能化的能力有：
	- （1）悬停提示（hover）
	- （2）转到定义（definition）
	- （3）查看引用（reference）
	- （4）1～3功能的跨文件（cross file）
	- （5）1～3功能的跨仓库（cross repository）
	- （6）1～3功能的跨语言（cross language）：
		- 例如 能够在Protobuf和生成的Java/Go Protobuf绑定之间导航，帮助他们找到相关的代码示例。
-
- 这些功能一般会在本地IDE中比较常见，特别是（1）～（4）的功能。
	- 也有用在 在线的 代码仓库平台、或者是 代码搜索平台 ，用于提供比较智能化的阅读体验。
	- 也有基于这些能力（或者是这些能力相关的底层能力），做 依赖分析、漏洞扫描 之类的 静态分析的功能。
-
- 这里主要看看相关技术与发展方向
	- [[LSP]]
	- [[LSIF]]
	- [[SCIP]]（灵感来自 SemanticDB）
	- [[Stack Graph]]（灵感来自Scope graph）
-
- 参考资料
	- stack-graph相关的文章、论文（github 的代码语义智能/代码导航（precise code navigation）实现方案）
		- [Introducing stack graphs - The GitHub Blog](https://github.blog/2021-12-09-introducing-stack-graphs/)
			- 一篇介绍
		- [https://dcreager.net/talks/stack-graphs/](https://dcreager.net/talks/stack-graphs/)
			- 会议分享video
		- [https://media.dcreager.net/dcreager-2022-ucsc-lsd-slides.pdf](https://media.dcreager.net/dcreager-2022-ucsc-lsd-slides.pdf)
			- 会议分享video相关的PPT
		- [GitHub - github/stack-graphs: Rust implementation of stack graphs](https://github.com/github/stack-graphs)
			- 相关源码
		- [https://news.ycombinator.com/item?id=29500602](https://news.ycombinator.com/item?id=29500602)
			- 作者在黑客论坛上的讨论
		- [https://arxiv.org/pdf/2211.01224.pdf](https://arxiv.org/pdf/2211.01224.pdf)
			- 相关发表的论文
		- [https://github.github.io/stack-graph-docs/](https://github.github.io/stack-graph-docs/)
			- 内部文档
		-
	- sourcegraph（专注于代码搜索/代码语义感知的公司，21年估值26.25亿美元）的方案
		- [Code navigation - Sourcegraph docs](https://docs.sourcegraph.com/code_navigation)
		- [Precise code navigation - Sourcegraph docs]
		- (https://docs.sourcegraph.com/code_navigation/explanations/precise_code_navigation)
		- [SCIP - a better code indexing format than LSIF](https://about.sourcegraph.com/blog/announcing-scip)
		- [https://techcrunch.com/2021/07/13/sourcegraph-raises-125m-series-d-on-2-6b-valuation-for-universal-code-search-tool/](https://techcrunch.com/2021/07/13/sourcegraph-raises-125m-series-d-on-2-6b-valuation-for-universal-code-search-tool/)
	- 国内的一些实践
		- [Alibaba Code代码索引技术实践：为Code Review提供本地IDE的阅读体验](https://mp.weixin.qq.com/s/7ZFezyneFADZ7_unAZWUEg)
		- 代码索引（阿里），用在他们的code review平台
		- [https://github.com/archguard/archguard](https://github.com/archguard/archguard)
			- 基于静态分析的架构治理（thoughtwork）
			- 作者最近去做代码AI的实践去了，利用了自己积累的静态分析的一些库
				- [LLM as Co-pilot：AutoDev 1.0 发布，开源全流程 AI 辅助编程](https://mp.weixin.qq.com/s/2qwx6l32_-P4TfLefMFbfg)
				- [Prompt 策略：代码库 AI 助手的语义化搜索设计](https://mp.weixin.qq.com/s/srV-fOoFvFRdYAvC2VIh0g)
				- [https://github.com/unit-mesh/auto-dev](https://github.com/unit-mesh/auto-dev)
					- 作者其他的一些相关项目
				- [https://github.com/modernizing/modernization](https://github.com/modernizing/modernization)
				- [Modernizing · GitHub](https://github.com/modernizing)
	- 其他相关的
		- 基于tree-sitter的重构工具
			- [https://github.com/ast-grep/ast-grep](https://github.com/ast-grep/ast-grep)
		- 基于tree-sitter的漏洞检测
			- [https://github.com/Bearer/bearer](https://github.com/Bearer/bearer)
-