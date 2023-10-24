# 依赖收集

每个属性都有自己的 dep 属性，存放他所依赖的watcher，当属性变化之后会通知自己对应的watcher去更新

默认会在初始化时调用render函数，此时会触发属性依赖收集 dep.depend

当属性发生修改时会触发watcher更新dep.notify()


依赖收集简版

```js
let obj = { name: 'poetry', age: 20 };

class Dep {
    constructor() {
      this.subs = [] // subs [watcher]
    }
    depend() {
      this.subs.push(Dep.target)
    }
    notify() {
      this.subs.forEach(watcher => watcher.update())
    }
}
Dep.target = null;
observer(obj); // 响应式属性劫持

// 依赖收集  所有属性都会增加一个dep属性，
// 当渲染的时候取值了 ，这个dep属性 就会将渲染的watcher收集起来
// 数据更新 会让watcher重新执行

// 观察者模式

// 渲染组件时 会创建watcher
class Watcher {
    constructor(render) {
      this.get();
    }
    get() {
      Dep.target = this;
      render(); // 执行render
      Dep.target = null;
    }
    update() {
      this.get();
    }
}
const render = () => {
    console.log(obj.name); // obj.name => get方法
}

// 组件是watcher、计算属性是watcher
new Watcher(render);

function observer(value) { // proxy reflect
    if (typeof value === 'object' && typeof value !== null)
    for (let key in value) {
        defineReactive(value, key, value[key]);
    }
}
function defineReactive(obj, key, value) {
    // 创建一个dep
    let dep = new Dep();

    // 递归观察子属性
    observer(value);

    Object.defineProperty(obj, key, {
        get() { // 收集对应的key 在哪个方法（组件）中被使用
            if (Dep.target) { // watcher
                dep.depend(); // 这里会建立 dep 和watcher的关系
            }
            return value;
        },
        set(newValue) {
            if (newValue !== value) {
                observer(newValue);
                value = newValue; // 让key对应的方法（组件重新渲染）重新执行
                dep.notify()
            }
        }
    })
}

// 模拟数据获取，触发getter
obj.name = 'poetries'

// 一个属性一个dep，一个属性可以对应多个watcher（一个属性可以在任何组件中使用、在多个组件中使用）
// 一个dep 对应多个watcher 
// 一个watcher 对应多个dep （一个视图对应多个属性）
// dep 和 watcher是多对多的关系

```

