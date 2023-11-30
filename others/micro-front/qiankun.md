# qiankun

## 资源加载

 qiankun 是通过 `fetch` 去获取微应用的引入的静态资源的，所以必须要求这些静态资源支持跨域。

### 图片、文件

原因是 qiankun 将外链样式改成了内联样式，但是字体文件和背景图片的加载路径是相对路径。

而 css 文件一旦打包完成，就无法通过动态修改 `publicPath` 来修正其中的字体文件和背景图片的路径。

* base64
* 绝对路径
  * 使用 CDN
  * 添加应用的绝对路径前缀

## JS 环境隔离

由于子应用访问的 window 对象是被 qiankun 代理后的对象

## CSS 样式隔离

* 沙箱隔离

  * `experimentalStyleIsolation`

    `experimentalStyleIsolation` 会在应用的所有 css 规则前加上一个特殊的选择

    ```js
    start({
      sandbox : { experimentalStyleIsolation: true }
    })
    ```

    效果：

    ```css
    // 假设应用名是 react16
    .app-main {
      font-size: 14px;
    }

    div[data-qiankun-react16] .app-main {
      font-size: 14px;
    }
    ```

* 样式前缀隔离

## 应用路由隔离

在子应用的路由中设置 `base`。

react 应用：

```js
<BrowserRouter basename={window.__POWERED_BY_QIANKUN__ ? '/app-react' : '/'}>
```

## 应用间通信

### 数据通信

主应用：

```js
import { initGlobalState, MicroAppStateActions } from 'qiankun';

// 初始化 state
const actions: MicroAppStateActions = initGlobalState(state);

actions.onGlobalStateChange((state, prev) => {
  // state: 变更后的状态; prev 变更前的状态
  console.log(state, prev);
});
actions.setGlobalState(state);
```

微应用：

```js
// 从生命周期 mount 中获取通信方法，使用方式和 master 一致
export function mount(props) {
  props.onGlobalStateChange((state, prev) => {
    // state: 变更后的状态; prev 变更前的状态
    console.log(state, prev);
  });

  props.setGlobalState(state);
}
```

### 路由跳转

由于基路由的存在，因此在微应用通过路由的 api 是无法实现应用间跳转的。有这几种办法可以跳转：

* `history.pushState()`
* 直接使用原生 `a` 标签写完整地址
* 修改 `location href` 跳转，如：`window.location.href = 'http://localhost:8080/app1'`
* 主应用提供操作路由的 Api

TODO: 可以使用面向对象、hooks、context 对其进行封装

TODO: 微应用间通信

## publicPath

```js
__webpack_public_path__ = window.__INJECTED_PUBLIC_PATH_BY_QIANKUN__;
```

[webpack 文档](https://webpack.js.org/guides/public-path/#on-the-fly)