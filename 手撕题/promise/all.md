`Promise.all()`核心逻辑：
1. 接受一个可迭代对象
2. 空对象，立即返回空数组
3. 所有 promise 成功，返回所有成功的结果（返回数组顺序与输入顺序一致）
4. 只要有一个 promise 失败，马上返回这个失败的结果

```javascript
function myPromiseAll(iterable) {
  return new Promise((resolve, reject) => {
    const promises = Array.from(iterable);
    if (promises.length === 0) {
      resolve([]);
    }

    const results = [];
    let resolvedCount = 0;

    promises.forEach((promise, index) => {
      Promise.resolve(promise)
        .then(result => {
          results[index] = result;
          resolvedCount++;

          if (resolvedCount === promises.length) {
            resolve(results);
          }
        })
        .catch(error => {
          reject(error);
        });
    });
  });
}
```