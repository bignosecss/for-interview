`apply` 与 `call` 的唯一区别就是参数
- apply 第二个参数接收数组，而 call 从第二个开始接收多个参数

```javascript
Function.prototype.myApply = function (context, args) {
  if (context === undefined || context === null) {
    context = window;
  }

  const uniqueKey = Symbol('fn');
  context[uniqueKey] = this;
  const result = context[uniqueKey](...(args || []));
  delete context[uniqueKey];

  return result;
}
```