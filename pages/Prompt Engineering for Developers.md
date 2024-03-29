- **TAG: [[ChatGPT]] [[Computer Science]]  **
- Two Types of large language models (LLMs)
- Principles of Prompting
	- 1. Write clear and specific instructions
		- Tactic 1: Use delimiters
			- example:
				- Triple quotes: """
				- Triple backticks: ```
				- Triple dashes: ---
				- Angle brackets: < >,
				- XML tags: <tag> </tag>
			- 用途
				- Avoiding Prompt Injections
					- 减少提示冲突
		- Tactic 2: Ask for structured output
			- HTML、JSON
		- Tactic 3: Check whether conditions are satisfied
			- Check assumptions required to do the task
		- Tactic 4: Few-shot prompting
			- Give successful examples of completing tasks
			- Then ask model to perform the task
	- 2. Give the model time to think
		- Tactic 1: Specify the steps to complete a task
			- example:
				- Step 1: ...
				- Step 2: ...
				- Step 3: ...
				- ...
				- Step N: ...
		- Tactic 2: Instruct the model to work out its own solution before rushing to a conclusion
			- 用途
				- 让模型多一点时间去自行思考解决方案，然后与我们给出的可能答案进行比较，为我们更好地校对问题
- Model Limitations
	- Hallucination
		- Makes statements that sound plausible but are not true
	- Reducing hallucinations
		- First find relevant information,
		- then answer the question base on the relevant information.
- Iterative Prompt Development
	- Prompt guidelines
		- Be clear and specific
		- Analyze why result does not give desired output.
		- Refine the idea and the prompt
		- Repeat
	- Iterative Process
		- Try something
		- Analyze where result does not give what you want
		- Clarify instructions, give more time to think
		- Refine prompt with a batch of examples
	- 由于不存在完美的、可用于所有场景的Prompt，所以需要有良好的工作模式，对每个场景，迭代出合适的prompt。
- 一些例子
	- Summarizing
		- 文章摘要、评论摘要...
		- 这里有意思的点是，可以让Prompt成为应用程序的一部分，让Prompt可以输出普通应用程序可以处理的数据格式，例如JSON，那么Prompt就替代了我们之前一般会去构建的各种专用机器学习模型。
	- Inferring
		- 情感识别、主题识别。。。
		- 同样的，不需要自己去训练一个机器学习模型，简单的zero-shot、few-shot，就可以利用Promtpt去完成同样的任务
	- Transforming
		- 翻译、文本格式替换、情感替换。。。
	- Expanding
		- temperature（温度）的运用
			- Temperature越高，则LLM在预测下一个词时，会更加随机
				- “随机”具体指的是，会选取概率较低的可选项
			- 所以在构建可预测回复的应用程序时，需要将Temperature设为0
		- 所以在一个应用程序的流程中，可能有些流程是Temperature较高的，有些流程是较低的，可以结合起来。
	- Chatbot
		- 上下文会作为输入
			- 上下文具体指的是 助手的回应，用户的提问、系统设置的助手人设
		- 在餐厅Chatbot中，在推荐用户菜品时，可以使用较高的Temperature。当提交订单给订单系统时，需要使用Temperature=0，使得提交订单的操作更准确、可预测
		- 用于生产时，需要处理/规避上下文的token限制问题