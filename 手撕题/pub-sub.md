发布订阅模式的核心逻辑：
1. 三个模块：发布者、消息中介、订阅者
2. 发布者与订阅者节藕，消息的安排由中介处理
3. 发布者只需要将消息发给中介，中介广播给所有的订阅者
4. 多对多的关系，但双方不必知道对方

```javascript
class EventEmitter {
  constructor() {
    this.events = {};
  }

  on(name, fn) {
    if (this.events[name]) {
      this.events[name].push(fn);
    } else {
      this.events[name] = [fn];
    }
  }

  emit(name, once = false, ...args) {
    if (!this.events[name]) return;

    const tasks = this.events[name];
    for (const t of tasks) {
      t(...args);
    }
    
    if (once) delete this.events[name];
  }

  off(name, fn) {
    if (!this.events[name]) return;

    const tasks = this.events[name];
    const index = tasks.findIndex(t => t === fn);
    if (index === -1) return;

    tasks.splice(index, 1);
  }

  once(name, fn) {
    const wrappedFn = (...args) => {
      fn(...args);
      this.off(name, wrappedFn);
    }

    this.on(name, wrappedFn);
  }
}
```