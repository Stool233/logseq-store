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
-
	-