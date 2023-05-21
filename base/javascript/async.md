# 异步

* setTimeout/setInterval
* ajax
* 事件

javascript 中的异步，

各种异步实现的差别，setTimeout、async await、promise、Generator


Generator 在于步进，可以完全地对流程进行控制。


## setTimeout/setInterval

## promise

## async await

## Generator

Generator 构造函数并不是全局可用。Generator 的实例必须从**生成器函数**返回：

```js
function* generator() {
  yield 1;
  yield 2;
  yield 3;
}

const gen = generator(); // "Generator { }"

console.log(gen.next().value); // 1
console.log(gen.next().value); // 2
console.log(gen.next().value); // 3
```

