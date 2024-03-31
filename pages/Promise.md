- Tags: [[异步]]
- 在编程中，`Promise` 和 `Future` 是两个与异步编程相关的概念，它们代表了一个异步操作的最终完成（或失败）及其结果值。这两个概念在不同的编程语言和库中可能有不同的实现和叫法，但它们的核心思想相似。下面我将分别解释它们通常指的是什么。
- ### Promise
  
  `Promise` 通常出现在JavaScript这样的异步编程环境中。一个`Promise`是一个代表了异步操作的最终完成或失败的对象。它允许你为异步操作的成功值或失败原因添加处理方法（handlers）。
  
  一个`Promise`对象有以下几种状态：
- **Pending**: 初始状态，既不是成功，也不是失败。
- **Fulfilled**: 意味着操作成功完成。
- **Rejected**: 意味着操作失败。
  
  `Promise`的基本用法如下：
  
  ```javascript
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
- ### Future
  
  `Future` 是在如Java、Dart等语言的异步编程中使用的一个概念，它类似于`Promise`，通常也表示一个可能还没有完成的异步计算的结果。
  
  在Java中，`Future` 提供了一种检查计算是否完成的方式，获取计算结果，以及取消计算的方法。一个`Future`对象代表了一个未来某时要返回的结果，当异步任务结束时，它将提供一个访问结果的方法。
  
  `Future`的基本用法（以Java为例）如下：
  
  ```java
  ExecutorService executor = Executors.newCachedThreadPool();
  Future<String> future = executor.submit(() -> {
    // 长时间运行的异步任务
    Thread.sleep(1000);
    return "Hello, World!";
  });
  
  // 在未来的某个时间获取结果
  try {
    String result = future.get(); // 可以阻塞等待结果
    System.out.println(result);
  } catch (InterruptedException | ExecutionException e) {
    e.printStackTrace();
  }
  
  executor.shutdown();
  ```
  
  在Dart和其他支持`Future`的语言中，`Future` 的概念和用法与JavaScript中的`Promise`相似。你可以在异步操作完成后通过`.then()`方法来获取结果，或者在Dart中使用`await`关键字等待`Future`的结果。
  
  不同语言中的`Promise`和`Future`都是处理异步操作的强大工具，它们提供了一种编写更清晰、更易于管理的异步代码的方法。