`Promise.race()`的核心逻辑
1. 接受一个可迭代对象作为参数
2. 返回一个 promise
3. 第一个结束的 promise 返回什么，race 就返回什么

```javascript
function myPromiseRace(iterable) {
  return new Promise((resolve, reject) => {
    const promises = Array.from(iterable);

    promises.forEach((promise) => {
      Promise.resolve(promise)
        .then(
          result => resolve(result),
          error => reject(error)
        );
    });
  });
}
```