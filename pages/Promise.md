- Tags: [[异步]]
- ### Promise
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
		  
		  在现代JavaScript中，`async/await` 语法允许以更同步的方式写异步代码，它背后仍然使用`Promise`。