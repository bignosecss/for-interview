`Promise.any()` 的核心逻辑：
1. 参数接受一个 **可迭代对象** （通常是 Promise 数组）
2. 只要有一个 Promise 成功，就立即返回这个成功的结果
3. 只有所有的 Promise 都失败（包括传递了一个空的可迭代对象），才返回一个 `AggregateError` 错误（包含所有失败原因）

```javascript
function myPromiseAny(iterable) {
  return new Promise((resolve, reject) => {
    // 处理参数，将可迭代对象转换为数组
    const promises = Array.from(iterable);
    // 空数组，直接报错
    if (promises.length === 0) {
      return reject(new AggregateError([], "All promises were rejected"));
    }

    // 记录失败的 Promise 数量和原因
    let rejectedCount = 0;
    const rejectionReasons = [];

    // 遍历所有 Promise 处理
    promises.forEach((promise, index) => {
      // 兼容非 Promise 类型的元素
      Promise.resolve(promise)
        .then(result => {
          // 只要有一个成功，立即 resolve 结果
          resolve(result);
        })
        .catch(error => {
          // 记录当前 Promise 失败原因
          rejectionReasons[index] = error;
          rejectedCount++;
          
          // 所有 Promise 都失败时，返回 AggregateError
          if (rejectedCount === promises.length) {
            return reject(new AggregateError(rejectionReasons, "All promises were rejected"));
          }
        });
    });
  });
}
```