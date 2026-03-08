`call` 的核心要点：
1. 在调用函数时，明确指定函数内部的 `this` 指向（上下文 context）
2. call 的参数是：context, arg1, arg2...，要展开
3. call 可以将一个对象的方法，借用给另一个对象调用
4. 立即执行！返回执行的结果

具体实现思路：
1. 将函数设置为另一个目标对象的属性
2. 立即执行该函数
3. 删除临时属性，返回函数执行的结果

```javascript
Function.prototype.myCall = function (context, ...args) {
  // 1. 处理上下文为空的情况
  if (context === undefined || context === null) {
    context = window;
  }

  // 2. 创建唯一键，避免覆盖目标对象原有属性
  const uniqueKey = Symbol('fn');
  // 3. 将 call 这个函数的 this 借给给目标对象，实现方法的借用
  // call 的 this 指向第一个对象的方法（调用时决定），这样才能实现方法借用
  context[uniqueKey] = this;
  // 4. 立即在另一个对象中执行当前函数
  const result = context[uniqueKey](...args);
  // 5. 删除临时属性
  delete context[uniqueKey];

  return result;
}
```