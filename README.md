# vue-eleme-webapp

## 在线预览
* 在线预览地址: https://mengwei09.github.io/vue-eleme
* PC端打开浏览器调试模式，并切换到移动设备模式
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

* 组合组件
> 组件意味着协同工作，通常父子组件会是这样的关系：组件 A 在它的模板中使用了组件 B。它们之间必然需要相互通信：父组件要给子组件传递数据，子组件需要将它内部发生的事情告知给父组件。然而，在一个良好定义的接口中尽可能将父子组件解耦是很重要的。这保证了每个组件可以在相对隔离的环境中书写和理解，也大幅提高了组件的可维护性和可重用性。

> 在 Vue 中，父子组件的关系可以总结为 **props down, events up**。父组件通过 **props** 向下传递数据给子组件，子组件通过 **events** 给父组件发送消息。看看它们是怎么工作的。

![props down, events up](https://cn.vuejs.org/images/props-events.png)

* **不应该**在子组件内部改变 prop, 如果你这么做了，Vue 会在控制台给出警告
> 注意在 JavaScript 中对象和数组是引用类型，指向同一个内存空间，如果 prop 是一个对象或数组，在子组件内部改变它**会影响**父组件的状态

* 如果把切换出去的组件保留在内存中，可以保留它的状态或避免重新渲染。为此可以添加一个 `keep-alive` 指令参数

* `v-if` vs `v-show`
> `v-if` 是“真正的”条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。
> `v-if` 也是**惰性的**：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。
> 相比之下，`v-show` 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。
> 一般来说，`v-if` 有更高的切换开销，而 `v-show` 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件不太可能改变，则使用 `v-if` 较好。

* 使用 `v-on` 绑定自定义事件
> 父组件可以在使用子组件的地方直接用 `v-on` 来监听子组件触发的事件
> 不能用 `$on` 侦听子组件释放的事件，而必须在模板里直接用 `v-on` 绑定，就像以下的例子：
```html
<div id="counter-event-example">
  <p>{{ total }}</p>
  <button-counter v-on:increment="incrementTotal"></button-counter>
  <button-counter v-on:increment="incrementTotal"></button-counter>
</div>
```
```js
Vue.component('button-counter', {
  template: '<button v-on:click="incrementCounter">{{ counter }}</button>',
  data: function () {
    return {
      counter: 0
    }
  },
  methods: {
    incrementCounter: function () {
      this.counter += 1
      this.$emit('increment')
    }
  },
})

new Vue({
  el: '#counter-event-example',
  data: {
    total: 0
  },
  methods: {
    incrementTotal: function () {
      this.total += 1
    }
  }
})
```

### CSS
* 关于Flex弹性布局, 参考[《CSS Mastery 3th edition》](http://www.apress.com/us/book/9781430258636)
> The Flexible Box Layout module, known as flexbox, is one of the newer sets of CSS properties we can use to
create layouts. It consists of a number of properties relating to both a container element (the flex container )
and its direct children ( flex items ), as well as how those child elements behave.

### ES6
* [Overview of ECMAScript 6 features](https://github.com/lukehoban/es6features#readme)
* [Understanding ECMAScript 6](https://github.com/nzakas/understandinges6)
* [ECMAScript 6 入门](http://es6.ruanyifeng.com/)

### Vue.js Template
* [Vue.js Template文档](http://vuejs-templates.github.io/webpack/)

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
