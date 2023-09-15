# Retry

The `retry` function is a utility method for executing promise functions with a retry mechanism.

## Documents in Other Language

- [中文文档](./README-zh.md)

## Key Features

- Promise: The `retry` function returns a Promise object that can be used to handle the final result or errors using the `.then()` and `.catch()` methods.
- Asynchronous Task: An asynchronous operation performed by the function passed to `promiseFn`.
- Retry Mechanism: In case of failure, the `retry` function automatically retries the task according to the specified retry count and interval to increase the chances of success.

## Usage

```javascript
retry(promiseFn, retryCount, retryIntervalSeconds, errorCallback);
```

## Parameters

- `promiseFn`: A callable Promise function that will be executed repeatedly until the maximum retry count is reached or it succeeds.
- `retryCount`: The maximum number of retries allowed.
- `retryIntervalSeconds`: The time interval in seconds between each retry.
- `errorCallback` (optional): A callback function called when the last retry fails. It receives an error object as a parameter and allows you to perform custom error handling logic. If not provided, the promise will be rejected with the original error.

## Installation

```sh
npm install @damo/retry
```

## Example

```javascript
const retry = require("@damo/retry");

// Define an asynchronous task
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

// Usage example
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

In the above example, we've added an `errorCallback` parameter as the fourth argument and handled it accordingly. When the last retry fails, the callback function is called with the error information as a parameter. You can handle custom error logic within the callback function and return the desired result.

Replace `simulateAsyncTask` with your actual asynchronous task function and adjust the values of `maxRetryCount`, `retryIntervalSeconds`, and `errorCallback` according to your requirements.
