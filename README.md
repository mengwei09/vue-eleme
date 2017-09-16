# vue-eleme-webapp

## [在线预览](https://mengwei09.github.io/vue-eleme)
* PC端打开浏览器调试模式，切换到移动设备模式
* 手机端直接用浏览器打开

## 技术栈
* [Vue 2.x](https://cn.vuejs.org/v2/guide/): 前端MVVM框架
* [vue-router 2.x](https://router.vuejs.org/zh-cn/): 前端路由
* [SASS](http://sass-lang.com/guide): CSS预处理器
* [better-scroll](https://github.com/ustbhuangyi/better-scroll): 移动端滚动插件
* [webpack](https://doc.webpack-china.org/): 现代 JavaScript 应用程序的模块打包器(module bundler)
* [axios](https://github.com/mzabriskie/axios): Promise based HTTP client for the browser and node.js
* ES6

## 🍔
### Vue
*  Vue **异步**执行 DOM 更新
> 只要观察到数据变化，Vue 将开启一个队列，并缓冲在同一事件循环中发生的所有数据改变。如果同一个 watcher 被多次触发，只会一次推入到队列中。这种在缓冲时去除重复数据对于避免不必要的计算和 DOM 操作上非常重要。然后，在下一个的事件循环“tick”中，Vue 刷新队列并执行实际 (已去重的) 工作。Vue 在内部尝试对异步队列使用原生的 `Promise.then` 和 `MutationObserver`，如果执行环境不支持，会采用 `setTimeout(fn, 0)` 代替

* **不应该**在子组件内部改变 prop, 如果你这么做了，Vue 会在控制台给出警告
> 注意在 JavaScript 中对象和数组是引用类型，指向同一个内存空间，如果 prop 是一个对象或数组，在子组件内部改变它**会影响**父组件的状态

* 如果把切换出去的组件保留在内存中，可以保留它的状态或避免重新渲染。为此可以添加一个 `keep-alive` 指令参数

### CSS
* 关于Flex弹性布局, 参考[《CSS Mastery 3th edition》](http://www.apress.com/us/book/9781430258636)
> The Flexible Box Layout module, known as flexbox, is one of the newer sets of CSS properties we can use to
create layouts. It consists of a number of properties relating to both a container element (the flex container )
and its direct children ( flex items ), as well as how those child elements behave.

### ES6
* [Overview of ECMAScript 6 features](https://github.com/lukehoban/es6features#readme)
* [Understanding ECMAScript 6](https://github.com/nzakas/understandinges6)
* [ECMAScript 6 入门](http://es6.ruanyifeng.com/)

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```
