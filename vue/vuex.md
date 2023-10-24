# Vuex

## Vuex 有哪几种属性？

有五种，分别是 State、 Getter、Mutation 、Action、 Module


| 属性 | 描述 |
| :--- | :--- |
| state | 基本数据(数据源存放地) |
| getters | 从基本数据派生出来的数据 |
| mutations | 提交更改数据的方法，同步 |
| actions | 像一个装饰器，包裹 mutations，使之可以异步。 |
| modules | 模块化 Vuex |

Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。

它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

Vuex 通过 `Vue.use(Vuex)` 进行注册，并通过全局混入的方式，将 `$store` 实例插入到每个组件中

```js
Vue.mixin({
  beforeCreate() {
    if (this.$options && this.$options.store) {
      //找到根组件 main 上面挂一个$store
      this.$store = this.$options.store
      // console.log(this.$store);

    } else {
      //非根组件指向其父组件的$store
      this.$store = this.$parent && this.$parent.$store
    }
  }
});
```

`getter` 的数据绑定原理和 vue 一样，通过 `Object.defineProperty` 来实现，给属性增加属性拦截器

一个 store 有如下属性：

* state

* mutations

* actions

* getters


vuex
1、什么是vuex？

2、使用vuex的核心概念

3、vuex在vue-cli中的应用

4、组件中使用 vuex 的值和修改值的地方？

5、在vuex中使用异步修改

6、pc端页面刷新时实现vuex缓存