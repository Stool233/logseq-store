- React 哲学
  collapsed:: true
	- 步骤一：将 UI 拆解为组件层级结构
		- 一开始，在绘制原型中的每个组件和子组件周围绘制盒子并命名它们。
	- 步骤二：使用 React 构建一个静态版本
		- 现在你已经拥有了你自己的组件层级结构，是时候实现你的应用程序了。
		- 最直接的办法是根据你的数据模型，构建一个不带任何交互的 UI 渲染代码版本…经常是先构建一个静态版本比较简单，然后再一个个添加交互。
			- 构建一个静态版本需要写大量的代码，并不需要什么思考;
			- 但添加交互需要大量的思考，却不需要大量的代码。
		- 构建应用程序的静态版本来渲染你的数据模型，
			- 将构建 [组件](https://zh-hans.react.dev/learn/your-first-component) 并复用其它的组件，
			- 然后使用 [props](https://zh-hans.react.dev/learn/passing-props-to-a-component) 进行传递数据。
				- Props 是从父组件向子组件传递数据的一种方式。
				- 如果你对 [state](https://zh-hans.react.dev/learn/state-a-components-memory) 章节很熟悉，不要在静态版本中使用 state 进行构建。
					- state 只是为交互提供的保留功能，即数据会随着时间变化。
					- 因为这是一个静态应用程序，所以并不需要。
			- 你既可以通过从层次结构更高层组件（如 `FilterableProductTable`）开始“自上而下”构建，也可以通过从更低层级组件（如 `ProductRow`）“自下而上”进行构建。
				- 在简单的例子中，自上而下构建通常更简单；
				- 而在大型项目中，自下而上构建更简单。
	- 步骤三：找出 UI 精简且完整的 state 表示
		- 为了使 UI 可交互，你需要用户更改潜在的数据结构。你将可以使用 **state** 进行实现。
		- 考虑将 state 作为应用程序需要记住改变数据的最小集合。
			- 组织 state 最重要的一条原则是保持它 [DRY（不要自我重复）](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)。
				- 计算出你应用程序需要的绝对精简 state 表示，按需计算其它一切。
		- 其中哪些是 state 呢？标记出那些不是的:
			- 随着时间推移 **保持不变**？如此，便不是 state。
			- 通过 props **从父组件传递**？如此，便不是 state。
			- 是否可以基于已存在于组件中的 state 或者 props **进行计算**？如此，它肯定不是state！
	- 步骤四：验证 state 应该被放置在哪里
		- 在验证你应用程序中的最小 state 数据之后，你需要验证哪个组件是通过改变 state 实现可响应的，或者 **拥有** 这个 state。
			- 记住：React 使用单向数据流，通过组件层级结构从父组件传递数据至子组件。
		- 为你应用程序中的每一个 state:
			- 1. 验证每一个基于特定 state 渲染的组件。
			- 2. 寻找它们最近并且共同的父组件——在层级结构中，一个凌驾于它们所有组件之上的组件。
			- 3. 决定 state 应该被放置于哪里:
				- 1. 通常情况下，你可以直接放置 state 于它们共同的父组件。
				- 2. 你也可以将 state 放置于它们父组件上层的组件。
				- 3. 如果你找不到一个合适来放这个 state 的地方，单独创建一个新的组件去管理这个 state，并将它添加到它们父组件上层的某个地方。
		- 用 [`useState()` Hook](https://zh-hans.react.dev/reference/react/useState) 为组件添加 state。
			- Hook 可以“钩住”组件的 [渲染周期](https://zh-hans.react.dev/learn/render-and-commit)。
	- 步骤五：添加反向数据流
		- 目前你的应用程序可以带着 props 和 state 随着层级结构进行渲染。但是为了支持通过用户输入来改变 state，你需要让数据反向传输。
			- React 使数据流变得明确，但比双向数据绑定需要多写一些代码。
		-
	-
- 添加交互
  collapsed:: true
	- 渲染和提交
		- 想象一下，你的组件是厨房里的厨师，把食材烹制成美味的菜肴。在这种场景下，React 就是一名服务员，他会帮客户们下单并为他们送来所点的菜品。这种请求和提供 UI 的过程总共包括三个步骤：
			- **触发** 一次渲染（把客人的点单分发到厨房）
			- **渲染** 组件（在厨房准备订单）
			- **提交** 到 DOM（将菜品放在桌子上）
		- 步骤 1: 触发一次渲染
			- 有两种原因会导致组件的渲染:
				- 组件的 **初次渲染。**
					- 当应用启动时，会触发初次渲染。
						- 框架和沙箱有时会隐藏这部分代码，但它是通过调用 [`createRoot`](https://zh-hans.react.dev/reference/react-dom/client/createRoot) 方法并传入目标 DOM 节点，然后用你的组件调用 `render` 函数完成的
				- 组件（或者其祖先之一）的 **状态发生了改变。**
					- 一旦组件被初次渲染，你就可以通过使用 [`set` 函数](https://zh-hans.react.dev/reference/react/useState#setstate) 更新其状态来触发之后的渲染。
						- 更新组件的状态会自动将一次渲染送入队列。（你可以把这种情况想象成餐厅客人在第一次下单之后又点了茶、点心和各种东西，具体取决于他们的胃口。）
		- 步骤 2: React 渲染你的组件
			- 在你触发渲染后，React 会调用你的组件来确定要在屏幕上显示的内容。**“渲染中” 即 React 在调用你的组件。**
				- **在进行初次渲染时,** React 会调用根组件。
				- **对于后续的渲染,** React 会调用内部状态更新触发了渲染的函数组件。
			- 这个过程是递归的：
				- 如果更新后的组件会返回某个另外的组件，那么 React 接下来就会渲染 *那个* 组件，而如果那个组件又返回了某个组件，那么 React 接下来就会渲染 *那个* 组件，以此类推。
				- 这个过程会持续下去，直到没有更多的嵌套组件并且 React 确切知道哪些东西应该显示到屏幕上为止。
			- 渲染必须始终是一次 [纯计算](https://zh-hans.react.dev/learn/keeping-components-pure):
				- **输入相同，输出相同。**
					- 给定相同的输入，组件应始终返回相同的 JSX。（当有人点了西红柿沙拉时，他们不应该收到洋葱沙拉！）
				- **只做它自己的事情。**
					- 它不应更改任何存在于渲染之前的对象或变量。（一个订单不应更改其他任何人的订单。）
		- 步骤 3: React 把更改提交到 DOM 上
			- 在渲染（调用）你的组件之后，React 将会修改 DOM。
				- **对于初次渲染**，React 会使用 [`appendChild()`](https://developer.mozilla.org/docs/Web/API/Node/appendChild) DOM API 将其创建的所有 DOM 节点放在屏幕上。
				- **对于重渲染**，React 将应用最少的必要操作（在渲染时计算！），以使得 DOM 与最新的渲染输出相互匹配。
			- **React 仅在渲染之间存在差异时才会更改 DOM 节点。**
		- 尾声：浏览器绘制
			- 在渲染完成并且 React 更新 DOM 之后，浏览器就会重新绘制屏幕。
				- 尽管这个过程被称为“浏览器渲染”（“browser rendering”），但我们还是将它称为“绘制”（“painting”），以避免在这些文档的其余部分中出现混淆。
		-
- 状态管理
	- Reducer和Context
	  collapsed:: true
		- 迁移状态逻辑至 Reducer 中
		  collapsed:: true
			- 对于那些需要更新多个状态的组件来说，过于分散的事件处理程序可能会令人不知所措。
			- 对于这种情况，你可以在组件外部将所有状态更新逻辑合并到一个称为 “reducer” 的函数中。
				- 这样，事件处理程序就会变得简洁，因为它们只需要指定用户的 “actions”。
			- **Reducer**，本质上是一个函数，负责根据“动作（action）”来集中处理**所有状态的变化逻辑**。
				- 通常配合 `useReducer` 钩子使用。
				- Reducer 会接收当前状态和 action，然后返回新的状态。
				- 这种模式让**复杂状态管理逻辑集中、清晰、可维护**。
			- 为什么要迁移到 Reducer？
				- **集中管理逻辑**：所有状态的变化都在 reducer 中描述，避免了逻辑分散在各个 handler 里。
				- **易于维护和扩展**：当状态逻辑变复杂时，只需修改 reducer。
				- **便于调试**：所有状态变更都有明确的 action 类型，易于追踪。
			- 例子
				- ```jsx
				  const initialState = { count: 0, name: '', error: null };
				  
				  function reducer(state, action) {
				    switch (action.type) {
				      case 'increment':
				        return { ...state, count: state.count + 1 };
				      case 'setName':
				        return { ...state, name: action.payload, error: null };
				      default:
				        return state;
				    }
				  }
				  
				  const [state, dispatch] = useReducer(reducer, initialState);
				  
				  function handleIncrement() {
				    dispatch({ type: 'increment' });
				  }
				  
				  function handleNameChange(e) {
				    dispatch({ type: 'setName', payload: e.target.value });
				  }
				  ```
			- useReducer
				- `useReducer` 这个 Hook 接收两个参数：reducer 函数和初始状态，返回一个数组：
					- ```js
					  const [state, dispatch] = useReducer(reducer, initialState);
					  ```
					- **state**：当前的状态值
					- **dispatch**：一个函数，用于“派发”一个 action，告诉 reducer 需要做什么类型的状态更新
						- `dispatch` 的用法
							- 你可以把 `dispatch` 理解为“**通知状态管理器，发生了什么事情**”。
								- 你调用 `dispatch(action)`，**action** 是一个对象，通常包含 `type` 字段（表示发生了什么）和其他 payload 数据。
								- React 会把当前 state 和 action 传给你定义的 reducer 函数，然后用 reducer 返回的新 state 更新组件。
							- 深入react源码
								- 每次调用 `dispatch(action)`，其实是往内部的“更新队列”里**添加一个 action**，并告诉 React 需要重新渲染。
								- React 在下一轮 render 时，会**用 reducer 和所有 action 计算出新 state**，并刷新组件。
		- 使用 Context 深层传递参数
		  collapsed:: true
			- 通常，你会通过 props 将信息从父组件传递给子组件。
			- 但是，如果要在组件树中深入传递一些 prop，或者树里的许多组件需要使用相同的 prop，那么传递 prop 可能会变得很麻烦。
			- Context 允许父组件将一些信息提供给它下层的任何组件，不管该组件多深层也无需通过 props 逐层透传。
				- **Context** 可以解决“**深层组件需要用到同样的数据**”的问题。
					- *Context* 允许你**不通过 props**，就能让某个数据从父组件“广播”到所有后代组件。
					- 这样，**中间层组件不需要再负责转传数据**，只有需要用的组件才“订阅”这个数据。
			- Context 的基本用法
				- ① 创建 Context
					- ```js
					  const UserContext = React.createContext();
					  ```
				- ② 提供数据（Provider）
					- 用 Provider 组件包裹需要访问数据的组件树，并传入 value：
						- ```jsx
						  <UserContext.Provider value="Tom">
						    <Parent />
						  </UserContext.Provider>
						  ```
					- 在 React 中，`Context.Provider` 允许你在组件树中向下传递一个“值”（`value`），让树下的任何子组件都可以方便地获取到这个值，而不需要一层层通过 props 传递。
						- value甚至可以是一个函数，例如useRecuder返回的dispatch
				- ③ 消费数据（Consumer 或 useContext）
					- 在任意深度的子组件中，直接获取：
						- ```jsx
						  function Profile() {
						    const user = React.useContext(UserContext);
						    return <div>{user}</div>;
						  }
						  ```
			- Context 适合的场景
				- **主题色、语言、用户信息、权限控制**等全局性数据。
				- 需要被**很多子组件**用到，且这些子组件**分布在组件树的不同位置**。
		-
	- 对 state 进行保留和重置
	  collapsed:: true
		- 状态与组件位置的关系
			- **每个组件的 state 是独立的**，React 通过组件在 UI 树中的位置来关联 state。
			- **同一个组件在不同位置渲染，会有不同的 state**；相同位置的相同组件会保留 state。
		- 什么时候 state 会被保留或重置？
			- 只有在**相同位置**渲染**相同类型**的组件时，state 会被保留。
			  collapsed:: true
				- **对 React 来说重要的是组件在 UI 树中的位置,而不是在 JSX 中的位置！**
					- **UI 树**是 React 内部维护的一个“组件树”，它记录了每个组件在页面结构中的层级和顺序。
						- **位置**指的是：某个组件在这棵树中的具体“地址”，
							- 比如“根组件的第一个子组件的第二个孙组件”等。
					- **JSX**只是我们写代码时的描述方式，是组件结构的声明（像 XML/HTML）。
						- **JSX 代码的位置**，只是你在代码文件里写的位置，而不是最终渲染出来时在树中的“实际地址”。
					- 为什么“UI 树位置”才重要？
						- React **根据组件在 UI 树中的位置来保存和关联 state**。
						- 只要每次渲染，某个位置出现了**相同类型的组件**，React 就会认为它是“同一个组件”，state 会被保留。
			- 如果组件被移除或换成不同类型的组件，state 会被销毁（重置）。
			- **结构变化**（比如 div 换成 section）会导致 state 重置。
		- key 的作用
			- 给组件设置 **key**，可以让 React 区分“同一位置的不同组件”，从而**强制重置 state**。
			- 常见于**列表渲染**中，但也适用于需要区分身份的场景（如聊天窗口、表单等）。
		- 常见陷阱
			- **不要将组件定义嵌套在另一个组件内部**，否则每次父组件渲染都会导致子组件是“新组件”，state 会重置。
			- 仅仅因为 JSX 结构变了，未必会导致 state 重置，关键看最终渲染树的结构和位置。
		- 强制重置 state 的两种方法
			- **方法一：将组件渲染在不同的位置**
				- 适合少量切换，直接用条件渲染实现。
					- 例如使用两个独立条件表达式来替换一个三元运算符
			- **方法二：使用 key**
				- 更通用，适合需要根据“身份”区分组件的场景。
					- 指定一个 `key` 能够让 React 将 `key` 本身而非它们在父组件中的顺序作为位置的一部分。
						- 请记住 key 不是全局唯一的。它们只能指定 **父组件内部** 的顺序。
					-
		- 在表单和复杂组件中的应用
			- **key 可以用于重置表单内容**，避免切换对象后保留了上一个对象的输入。
			- 但如果想**保留多个对象的输入内容**，可以将 state 提升到父组件，或用 localStorage 等持久化方案。
- 脱围机制
	- 使用 ref 引用值
		- 当你希望组件“记住”某些信息，但又不想让这些信息 [触发新的渲染](https://zh-hans.react.dev/learn/render-and-commit) 时，你可以使用 **ref** 。
	- 使用 Effect 进行同步
		- 什么是 Effect，它与事件（event）有何不同？
			- 在接触 Effect 之前，你需要熟悉 React 组件中的两种逻辑类型：
				- **渲染代码**（在 [描述 UI](https://zh-hans.react.dev/learn/describing-the-ui) 中有介绍）位于组件的顶层。
					- 你在这里处理 props 和 state，对它们进行转换，并返回希望在页面上显示的 JSX。
					- [渲染代码必须是纯粹的](https://zh-hans.react.dev/learn/keeping-components-pure)——就像数学公式一样，它只应该“计算”结果，而不做其他任何事情。
				- **事件处理程序**（在 [添加交互性](https://zh-hans.react.dev/learn/adding-interactivity) 中有介绍）是组件内部的嵌套函数，它们不光进行计算, 还会执行一些操作。
					- 事件处理程序可能会更新输入字段、提交 HTTP POST 请求来购买产品，或者将用户导航到另一个页面。
					- 事件处理程序包含由特定用户操作（例如按钮点击或输入）引起的“副作用”（它们改变了程序的状态）。
			- **Effect 允许你指定由渲染自身，而不是特定事件引起的副作用**。
				- 在聊天中发送消息是一个“事件”，因为它直接由用户点击特定按钮引起。然而，建立服务器连接是一个 Effect，因为无论哪种交互致使组件出现，它都应该发生。
			- Effect 在 [提交](https://zh-hans.react.dev/learn/render-and-commit) 结束后、页面更新后运行。
				- 此时是将 React 组件与外部系统（如网络或第三方库）同步的最佳时机。
		- 你可能不需要 Effect
			- **不要急着在你的组件中使用 Effect**。记住，Effect 通常用于暂时“跳出” React 并与一些 **外部** 系统进行同步。这包括浏览器 API、第三方小部件，以及网络等等。
			- 如果你的 Effect 只是根据其他状态来调整某些状态，那么 [你可能并不需要一个 Effect](https://zh-hans.react.dev/learn/you-might-not-need-an-effect)。
		- 如何编写 Effect
			- 要编写一个 Effect, 请遵循以下三个步骤：
				- 1. **声明 Effect**。通常 Effect 会在每次 [提交](https://zh-hans.react.dev/learn/render-and-commit) 后运行。
				- 2. **指定 Effect 依赖**。大多数 Effect 应该按需运行，而不是在每次渲染后都运行。例如，淡入动画应该只在组件出现时触发。连接和断开服务器的操作只应在组件出现和消失时，或者切换聊天室时执行。你将通过指定 **依赖项** 来学习如何控制这一点。
			-
			-