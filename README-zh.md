# Retry

retry 函数是一个实用工具方法，用于执行带有重试机制的 Promise 函数。

## 其他语言文档

- [Document in english](./README.md)

## 关键功能

- Promise：retry 函数返回一个 Promise 对象，可以使用 .then() 和 .catch() 方法来处理最终结果或错误。
- 异步任务：通过传递给 promiseFn 的函数来执行的异步操作。
- 重试机制：当异步任务失败时，retry 函数会根据指定的重试次数和时间间隔自动尝试重新执行该任务，以增加成功的几率。

## 调用方式

```javascript
retry(promiseFn, retryCount, retryIntervalSeconds, errorCallback);
```

## 参数

- promiseFn：一个可执行的 Promise 函数。它将被重复执行，直到达到最大重试次数或成功为止。
- retryCount：重试的次数限制。即最大重试次数。
- retryIntervalSeconds：每次重试之间的秒数间隔。
- errorCallback（可选）：在最后一次重试失败时调用的回调函数。该函数接收一个错误对象作为参数，并允许您执行自定义的错误处理逻辑。如果未提供 errorCallback，则会以原始错误对象进行拒绝。

## 安装

```sh
npm install @damo/retry
```

## 使用

```javascript
const retry = require("@damo/retry");

// 定义异步方法
function simulateAsyncTask() {
  return new Promise((resolve, reject) => {
    const isSuccess = Math.random() < 0.5; // 50% success rate

    if (isSuccess) {
      resolve("Operation successful!");
    } else {
      reject(new Error("Operation failed!"));
    }
  });
}

// 使用样例
const maxRetryCount = 3;
const retryIntervalSeconds = 2;

retry(simulateAsyncTask, maxRetryCount, retryIntervalSeconds, (error) => {
  console.error("Retry failed:", error.message);
  return "Custom error handling result";
})
  .then((result) => {
    console.log("Final result:", result);
  })
  .catch((error) => {
    console.error("Retry failed:", error.message);
  });
```

在上述示例中，我们添加了一个 errorCallback 参数作为第四个参数，并进行相应的处理。当最后一次重试仍然失败时，会调用该回调函数，并将错误信息作为参数传递给它。您可以在回调函数中处理自定义的错误逻辑，并返回相应的结果。

请根据需要将示例中的 simulateAsyncTask 替换为实际的异步任务函数，并根据具体情况调整 maxRetryCount、retryIntervalSeconds 和 errorCallback 的值。
