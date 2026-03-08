`instanceof` 的核心逻辑：
1. 要确保比较的是对象或函数
2. 如果是 primitive types 直接返回 false
3. 比较的实际上是原型，遍历左侧对象的隐式原型(__proto__)链中，是否能够找到右侧函数的显式原型(prototype)

```javascript
function myInstanceof(obj, fn) {
  if (typeof obj !== 'object' && typeof obj !== 'function') return false;

  let proto = Object.getPrototypeOf(obj);
  while (proto !== null) {
    if (proto === fn.prototype) return true;
    proto = Object.getPrototypeOf(proto);
  }

  return false;
}
```