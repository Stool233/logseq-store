- Frame
	- #### Frame 的定义
		- 在 Figma 中，Frame 可以被看作是容器，它可以包含图形、文本、图标等其他设计元素。Frame 的功能与 Adobe XD 的画板或 Photoshop 的图层组类似，但它提供了更多灵活的布局和交互功能。
	- **主要功能包括：**
		- **容纳元素**：可以将多个设计元素（如图形、文本和图标）放入一个 Frame 中。
		- **布局属性**：Frame 支持灵活的布局设置，如自动布局，允许设计师快速调整和规模设计元素。
		- **响应式设计**：通过约束和缩放选项，Frame 中的元素可以响应不同的屏幕尺寸和设备。
		- **嵌套**：Frame 可以嵌套其他 Frame，这样可以创建复杂的界面层次结构。
		- **组件化**：Frame 可以转换为组件，实现设计的重用和一致性，非常适合创建交互式原型和用户界面库。
- Layers
	- ### Figma 中的 Layers（图层）
		- 在 Figma 中，**Layers（图层）** 是设计和组织界面元素的基本结构。它们使设计师能够以层叠的方式控制和管理界面中的每个元素，包括框架、形状、文本、图片等。
		- #### 图层的基本概念
			- 图层在 Figma 的界面中通常显示在左侧的图层面板中。每当你添加一个新的元素（如形状、文本或图片）到画布上时，Figma 会自动为其创建一个新的图层。这些图层可以被重命名、重排、隐藏或锁定，以便更好地管理设计。
		- #### 图层的主要功能
			- **组织**：图层可以帮助设计师管理画布上的元素。通过对图层进行排序和分组，可以控制元素的显示顺序和层次结构。
			- **选择与编辑**：通过在图层面板中选择特定图层，可以快速定位和编辑画布上的元素。
			- **显示/隐藏**：图层可以被隐藏或显示，这对于处理复杂的设计中的不同部分特别有用。
			- **锁定/解锁**：可以锁定图层以防止不小心的编辑或移动。
			- **查找与过滤**：当项目非常庞大时，可以使用图层面板上方的搜索框来快速找到特定的图层。
		- #### 使用图层的技巧
			- **命名规范**：给图层命名时使用清晰和一致的命名规范，可以帮助团队成员更容易理解和找到需要的图层。
			- **分组**：通过将相关的图层分组（选择多个图层并按 `Ctrl + G` 或 `Cmd + G`），可以创建更清晰的层次结构，使得管理更为方便。
			- **使用图层顺序**：在 Figma 中，图层列表的顺序决定了元素在画布上的堆叠顺序。位于列表顶部的图层会显示在最上面。
			- **利用隐藏与锁定**：在处理复杂的设计时，隐藏不相关的图层或锁定重要的图层，可以减少误操作，并专注于当前正在编辑的部分。
		- #### 实际应用
			- 在实际设计过程中，合理地利用图层可以极大地提升工作效率和设计的组织性。例如，在设计一个网页的过程中，可以将头部、内容和底部区域分别放在不同的图层组中。这样，当需要对特定区域进行修改时，可以快速定位并只显示该区域的图层，从而简化编辑过程。
		- 总的来说，图层是 Figma 中管理和控制设计元素的重要工具，适当地使用图层功能可以帮助设计师更有效地创建、编辑和共享他们的设计作品。
-
- 在 Figma 中，**Frame（框架）** 和 **Group（组）** 是两种常用的元素组织方式，它们都允许设计师将多个对象组合在一起，但它们的功能和用途存在一些重要的区别。理解这些差异有助于在设计过程中更有效地使用这些工具。
	- ### Frame（框架）
		- Frame 在 Figma 中具有高度的功能性和灵活性，可以看作是一个功能丰富的容器。它不仅可以组合元素，还具有自己的属性和布局功能。
		- **主要特点包括：**
		- **布局属性**：Frame 支持自动布局，允许设计师对内部元素进行灵活的排列和对齐，例如设置填充、间距和分布方式。
		- **尺寸和约束**：Frame 允许设定特定的尺寸，并对内部元素设置约束，这有助于创建响应式设计。
		- **嵌套功能**：Frame 可以嵌套其他 Frame，创建复杂的界面层次结构。
		- **组件化**：Frame 可以转换成组件，用于创建可重用的设计元素，这有助于保持设计的一致性和减少重复工作。
	- ### Group（组）
		- Group 是一种更基础的组织方式，主要用于简单地将多个元素组合在一起，以便于同时移动、调整或应用属性。
		- **主要特点包括：**
			- **简单组合**：Group 主要用于逻辑上将元素组合在一起，没有 Frame 的多种布局和尺寸属性。
		- **无额外属性**：Group 不像 Frame 那样可以设置自动布局或其他复杂的布局属性。它只是一个简单的元素集合，不影响内部元素的布局和尺寸。
		- **操作便利**：使用 Group 可以快速选择和移动一组元素，但对于需要细致控制布局的场景，Group 的功能可能显得有限。
	- ### 使用场景比较
		- **使用 Frame**：当你需要对内部元素进行精细的布局控制，或者打算创建**可以复用的组件**时，应该使用 Frame。例如，设计一个按钮，其中包含图标和文字，并且这些元素需要有精确的对齐和填充，Frame 是更好的选择。
		- **使用 Group**：当你只需要将几个元素临时绑定在一起，以便于移动或调整它们，而不需要关心布局或响应式设计时，使用 Group 更合适。例如，如果你有一个图标和一段文本仅仅需要在画布上一起移动，那么 Group 就足够了。
	- 总的来说，选择 Frame 还是 Group 取决于你的具体需要——是否需要布局控制、是否要创建响应式设计元素，以及是否计划将组合的元素用作更复杂的设计结构的一部分。
-
- Plugin
	- Stark
	  collapsed:: true
		- Check Contrast
			- ### Figma 插件 Stark：检查对比度功能介绍
				- **Stark** 是一款可以在 Figma 中使用的插件，专注于提升设计的可访问性，其中一个重要的功能就是检查颜色对比度。
				- #### 功能概述
					- Stark 插件的 **Check Contrast（检查对比度）** 功能允许设计师快速检测他们的设计中文本与背景之间的颜色对比度是否符合无障碍标准。这是确保所有用户，特别是视觉障碍用户，可以清晰阅读界面上的信息的关键一步。
				- #### 如何使用 Stark 检查对比度
					- 1. **安装插件：**
						- 在 Figma 内部，通过 “插件” 菜单搜索 “Stark”，然后选择安装。
					- 2. **使用检查对比度功能：**
						- 在 Figma 的设计文件中，选择你想要检查对比度的元素。
						- 打开 Stark 插件，选择 “Check Contrast”。
						- 插件会分析选定元素的前景色和背景色，并显示其对比度比率。
					- 3. **解读结果：**
						- Stark 会根据 [[WCAG]]（Web Content Accessibility Guidelines，网页内容可访问性指南）的标准来评估对比度。
						- 它将提供对比度比值，并指出该比值是否满足 AA 或 AAA 级别的标准（AA 为最低推荐标准，AAA 为更高标准）。
				- #### 为什么对比度检查很重要
					- **提高可访问性：** 保证所有用户，特别是视力不佳的用户，都能舒适地阅读和理解内容。
					- **遵守法规：** 许多国家和地区有法律要求数字产品必须可访问，对比度检查是符合这些法规的一个重要步骤。
					- **改善用户体验：** 良好的对比度不仅有助于视觉障碍者，它也能提升整体的用户体验，使内容对所有人都更易读。
				- #### 结论
					- Stark 插件的 Check Contrast 功能是 Figma 用户在设计过程中一个极具价值的工具，它帮助设计师确保他们的产品不仅外观美观，而且对所有用户都是可访问的。通过简单的几步操作，设计师可以快速检测并调整设计，确保它们满足必要的可访问性标准。
-
- Figma interactions可以配置变量、以及各种判断条件
	- 可以模拟出各种复杂的用户流程
-
- Figma DEV模式
	- 适合从开发者角度理解设计/原型图
	- 生成了从设计转成代码相关的一些CSS配置与预览，以及方便assert的下载，便于开发。
	-
-
- Figma vscode插件
	- https://marketplace.visualstudio.com/items?itemName=figma.figma-vscode-extension
	- 用途：
		- **实时查看并回复评论和活动**
		- **根据设计获取代码建议**
			- 根据您正在积极参与的设计进行自动完成，保持流程顺畅并比以往更加高效。
				- 在设计中选择一个图层，当您键入时，属性将显示为代码建议
				- 只需单击一下即可将资产下载到您的存储库
		- **将代码文件链接到设计组件**
			- 通过保持设计系统和代码库同步，帮助您的整个团队更快地行动。快速导航并记录与设计文件相关的代码库，不再担心查找组件的现有实现。
			- 添加和查看开发资源链接以轻松参考文档
		- **运行开发模式插件来定制您的体验**
			- 使用插件在开发模式下扩展工作流程和代码输出，而无需离开 VS Code。
				- 运行 codegen 插件来根据您正在使用的任何框架或您的设计系统定制代码片段。
				- 连接到 Github、Jira 和 Storybook 等工具，以保持跨事实来源的同步。
-
-