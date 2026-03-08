`bind` 返回一个函数，作用是将调用的函数的 `this` 永久绑定在指定的上下文上。

```javascript
Function.prototype.myBind = function (context, ...args) {
  const self = this;
  return function (...newArgs) {
    return self.apply(context, [...args, ...newArgs]);
  }
}
```