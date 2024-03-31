- Tags: [[异步编程]]
- `Promise` 通常出现在JavaScript这样的异步编程环境中。一个`Promise`是一个代表了异步操作的最终完成或失败的对象。它允许你为异步操作的成功值或失败原因添加处理方法（handlers）。
- 一个`Promise`对象有以下几种状态：
	- **Pending**: 初始状态，既不是成功，也不是失败。
	- **Fulfilled**: 意味着操作成功完成。
	- **Rejected**: 意味着操作失败。
- `Promise`的基本用法如下：
	- ```javascript
	  let promise = new Promise(function(resolve, reject) {
	    // 异步操作的代码
	  
	    if (/* 操作成功 */) {
	        resolve(value); // 若操作成功，调用resolve并传入结果
	    } else {
	        reject(error); // 若操作失败，调用reject并传入错误对象
	    }
	  });
	  
	  // 使用.then()方法处理成功和失败
	  promise.then(
	    function(value) { /* 成功时执行 */ },
	    function(error) { /* 失败时执行 */ }
	  );
	  ```
- 在现代JavaScript中，`async/await` 语法允许以更同步的方式写异步代码，它背后仍然使用`Promise`。
- `Promise` 在 JavaScript 中扮演了极其重要的角色。其主要作用包括但不限于以下几点：
	- 1. **异步编程的简化**: `Promise` 提供了一种更简洁而强大的方式来进行异步编程。通过使用 `Promise`，开发者可以避免所谓的“[[回调地狱]]”（callback hell），这是指嵌套多层回调函数而导致代码难以理解和维护的情况。
	- 2. **链式调用**: `Promise` 支持链式调用（thenable），允许开发者以接近同步代码的方式书写异步操作，通过 `.then()` 和 `.catch()` 方法来顺序执行异步任务，并在每个步骤处理成功或失败的结果。
	- 3. **状态管理**: `Promise`对象代表一个异步操作的最终完成（或失败）及其结果值。
		- 它有三种状态：pending（等待中），fulfilled（已成功），和 rejected（已失败）。
		- 这个状态模型允许开发者编写更健壮的代码来处理异步操作的多种可能性。
	- 4. **错误处理**: `Promise` 提供了 `.catch()` 方法来集中处理异步操作中可能出现的错误，这样开发者可以更容易地管理异常。
	- 5. **并行控制**: Promises 可以与 `Promise.all()` 方法配合使用，允许并行执行多个异步操作，并等待所有操作完成。
	- 6. **流程控制**: `Promise` 可以用来控制异步流程，例如，通过 `Promise.race()` 方法可以实现竞态，即多个异步操作中只取最快返回的结果。
	- 7. **更好的可测试性**: `Promise` 使得异步代码的测试变得更加容易。
		- 测试框架可以返回一个 `Promise`，并在 `Promise` 解决后继续进行断言。
	- 8. **集成异步函数**: 在 ES2017 引入的 `async/await` 语法中，`Promise` 被用作 `await` 关键字的基础，这使得异步函数更加易读和易写。
- 示例代码：
	- ```javascript
	  function asyncOperation() {
	    return new Promise((resolve, reject) => {
	        // 模拟异步操作
	        setTimeout(() => {
	            const result = true; // 假设这是异步操作的结果
	            if (result) {
	                resolve('Operation successful');
	            } else {
	                reject('Operation failed');
	            }
	        }, 1000);
	    });
	  }
	  
	  asyncOperation().then((message) => {
	    console.log(message); // 当操作成功时执行
	  }).catch((error) => {
	    console.error(error); // 当操作失败时执行
	  });
	  ```
- 通过使用 `Promise`，上述代码展示了如何执行异步操作并在完成时处理结果或错误。这种模式在现代 web 开发中非常常见，用于处理 HTTP 请求、文件操作、数据库交互等异步任务。
-
- 一个简单的go的实现
	- https://github.com/Stool233/snippets-hub/tree/main/go-simple-promise
- 复杂一点的go实现
	- https://github.com/Stool233/snippets-hub/tree/main/go-promise
-
	-
	-