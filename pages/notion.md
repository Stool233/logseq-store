- 相关概念/功能
	- notion relation
		- 可以对两个表格进行关联
			- 方便整理数据关联，做快捷访问关联数据、针对关联数据做跟踪统计等功能
		- 也可以在同个表格中做关联
			- 例如实现父子关系
		- 将“关系数据库”的一些概念提供给非程序员
	- formula
		- 可以提供公式、或者内置函数，完成更灵活的功能
	- button
		- 提供了更快捷的变更数据的方式
-
- 一些建议
	- 需要图片管理时，不要使用notion的图片文件管理，可以直接把图片上传到s3/r2/oss，然后用链接的方式做关联管理。避免notion的性能/网络问题
		- 如果有自动化的方式就更好了
			- 例如先利用notion的图片文件管理，后面定期迁移到对象存储
	- 可以尝试用大表结合view时hidden部分字段+Filter的方式，只维护少数几个数据库，方便做管理。减少数据库多时，误操作的可能性
	- 如何设计自己的notion项目
		- 分析模板
			- 我们在拿到任何模板的时候
			- 我们需要做的第一件事情
			- 那就是我们需要分析这个模板
				- 用到了什么样的数据库
				- 用到了什么样的页面
				- 这个数据库用到了
					- 什么样的view
					- 什么样的property
- 想法
	- 数据相关
		- 数据组织
			- 主要以表格/数据库的方式
		- 数据管理
			- 录入、变更
		- 数据展示
			- pages、block
				- 类似web前端展示的组织方式
	- 是否能与logseq做结合
	-
-
	-