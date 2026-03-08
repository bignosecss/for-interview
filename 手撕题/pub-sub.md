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

  emit(name, ...args) {
    if (!this.events[name]) return;

    // 复制一份，防止在回调中 off 影响遍历
    const tasks = [...this.events[name]];
    for (const t of tasks) {
      t(...args);
    }
  }

  off(name, fn) {
    if (!this.events[name]) return;
    if (!fn) {
      delete this.events[name];
      return;
    }

    this.events[name] = this.events[name].filter(f => f !== fn);

    if (this.events[name].length === 0) {
      delete this.events[name];
    }
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