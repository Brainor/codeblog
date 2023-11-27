---
title: javascript 学习
tags: javascript
date: 2023-10-16 12:41:00 +8000
categories: coding
---

## 参考
[MDN 文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference)

## 语句, 对象
### [import](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/import) 与 [export](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/export)
ECMAScript 模块使用 `import` 来导入模块，使用 `export` 来导出模块。
静态的 import 语句用于导入由另一个模块导出的绑定。  
export 语句用于从模块中导出实时绑定的函数、对象或原始值，以便其他程序可以通过 import 语句使用它们。
```javascript
function cube(x) {
  return x * x * x;
}

const foo = Math.PI + Math.SQRT2;

var graph = {
  options: {
    color: "white",
    thickness: "2px",
  },
  draw: function () {
    console.log("From graph draw function");
  },
};

export { cube, foo, graph };
```
{: file='my-module.js'}
```javascript
import { cube, foo, graph } from "my-module.js";

graph.options = {
  color: "blue",
  thickness: "3px",
};

graph.draw();
console.log(cube(3)); // 27
console.log(foo); // 4.555806215962888
```
`import.meta.url`: The full URL to the module, includes query parameters and/or hash (following the `?` or `#`). 

### [require](https://nodejs.org/api/modules.html#requireid) 与 [module.exports](https://nodejs.org/api/modules.html#moduleexports)
CommonJS 使用 `require` 来导入模块，使用 `module.exports` 或 `exports` 来导出模块。
```javascript
const EventEmitter = require('node:events');

module.exports = new EventEmitter();

// Do some work, and after some time emit
// the 'ready' event from the module itself.
setTimeout(() => {
  module.exports.emit('ready');
}, 1000); 
```
{: file='a.js'}
```javascript
exports.a = 12;
module.exports.b = 13;
```
{: file='b.js'}
```javascript
const a = require('./a');
a.on('ready', () => {
  console.log('module "a" is ready');
}); 
const b = require('./b');
console.log(b.a); // 12
```

### [Promise](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)
Promise 对象用于表示一个异步操作的最终完成 (或失败), 及其结果值.  
`Promise((resolve, reject)=>{...}).then(resolve, reject).catch(reject)`

[Promise 构造函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/Promise)  
`new Promise(executor)`, `executor = (resolveFunc, rejectFunc)=>{...}`
- `resolveFunc(value)`的参数可以为 promise 对象, 则`resolveFunc(value)`的状态(`fulfilled`, `rejected`)与该 promise 对象的状态相同.  
- `executor`的返回值被忽略
- `executor`中如果有 error 出现, 则 promise 对象`rejected`.
- 当先调用`resolveFunc`, `value`被`resolved`. promise 对象可以是 pending(传入了 thenable), 可以是 fulfilled(传入了非 thenable), 也可以是 rejected(传入了 invalid resolution value).
- 当先调用`rejectFunc`, promise 对象被 rejected
- 当 promise settles 后, 异步调用通过`then`, `catch`, `finally`关联的 handlers, 将fulfillment value 或 rejection reason 作为输入参数传给它们.

[Promise.prototype.then()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then)
`promise.then(onFulfilled, onRejected)`, `onFulfilled = (value)=>{...}`, `onRejected = (reason)=>{...}`
立即返回一个 promise 对象, 当 handler 函数
- 返回一个值/不返回值, promise 对象 fulfilled, value=value/`undefined`
- 出错, promise 对象 rejected, reason=error
- 返回一个 promise 对象, promise 对象的状态与返回的 promise 对象的状态相同, value/reason 与返回的 promise 对象的 value/reason 相同

#### [async](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/async_function) 和 [await](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/await)
await 返回从 `Promise` 实例或 thenable 对象取得的处理结果, 它只能在 async 函数或者模块顶层中使用。

### [IIFE](https://developer.mozilla.org/zh-CN/docs/Glossary/IIFE)
立即调用的函数表达式(Immediately Invoked Function Expression)是一个在定义时就会立即执行的 JavaScript 函数.  
```javascript
(function () {
  statements
})();
```

### [展开语法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Spread_syntax)
```javascript
function sum(x, y, z) {
  return x + y + z;
}

const numbers = [1, 2, 3];

console.log(sum(...numbers));
// Expected output: 6

console.log(sum.apply(null, numbers));
// Expected output: 6
```

### [解构赋值](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)
```javascript
const [y, z] = [1, 2, 3, 4, 5]; // y = 1, z = 2
const { b = 2 } = { b: undefined }; // b is 2
const { a, ...others } = { a: 1, b: 2, c: 3 }; // a = 1, others = { b: 2, c: 3 }
const { p: foo, q: bar, z: zz = 12 } = { p: 42, q: true }; // foo = 42, bar = true, zz = 12
```
### [可选链运算符](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Optional_chaining)
用 optional chaining 获取对象的属性或调用方法时, 若属性或方法不存在, 则返回 undefined.
```javascript
const adventurer = {
  name: 'Alice',
  cat: {
    name: 'Dinah'
  }
};
const dogName = adventurer.dog?.name; // undefined
console.log(adventurer?.["dog"]); //undefined
console.log(adventurer.someNonExistentMethod?.()); // undefined
```

### [MutationObserver](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserver)
当DOM中的元素发生改变时会执行函数
```javascript
const observer = new MutationObserver(function(mutations, observer) {
  mutations.forEach(function(mutation) {
    console.log(mutation.type);
  });
  observer.disconnect();
});
```

## 网页搭建
1. [npm](https://docs.npmjs.com/cli/v10/commands/npm-install)
    ```bash
    npm init -y
    npm install webpack webpack-cli --save-dev

    npm install lodash # 将使用的第三方包放入本地目录
    npm install --save-dev style-loader css-loader # 将开发用的第三方包放入本地目录
    ```
2. [webpack](https://webpack.js.org/guides/getting-started)  
打包工具
    ```powershell
    npm install -g npm@10.2.0 # 更新npm
    npm config set fund false
    npm init -y
    npm install webpack webpack-cli --save-dev
    ```
3. [node.js](https://nodejs.org/api/modules.html)  
webpack 运行在 Node.js 环境中

## [Vue.js](https://cn.vuejs.org/guide/introduction.html)
用于构建用户界面的 JavaScript 渐进式框架
```html
<script type="importmap">
    {
      "imports": { "vue": "https://unpkg.com/vue@3/dist/vue.esm-browser.js" }
    }
</script>
<div id="app">
    <button @click="count++">{{ count }}</button>
    <div v-bind="objectOfAttrs"></div>
    <button @click="increment">{{ count_2 }}</button>
</div>
<div id="app2">{{ message }}</div>
<script type="module" src="app.js"></script>
<script type="module">
    import { createApp, ref } from 'vue'
    createApp({
        setup() {
            const message = ref('Hello Vue!')
            return { message }
        }
    }).mount('#app2')
</script>
```
```javascript
import { ref, createApp } from 'vue'
const count_2 = ref(0)
function increment() { count_2.value++; }
const app = createApp({
    data() {
        return {
            count: 0, count_2, increment,
            objectOfAttrs: { id: 'container', class: 'wrapper' }
        }
    }
})
app.mount('#app')
```
{: file='app.js'}

## Chrome插件
1. [Chrome API reference](https://developer.chrome.com/docs/extensions/reference/), [Extension development overview](https://developer.chrome.com/docs/extensions/mv3/devguide/)
2. [Edge extensions](https://learn.microsoft.com/en-us/microsoft-edge/extensions-chromium/)
3. [Chrome extensions 入门](https://developer.chrome.com/docs/extensions/mv3/getstarted/), [chrome插件开发攻略](http://blog.haoji.me/chrome-plugin-develop.html)
4. [Manifest V2](https://developer.chrome.com/docs/extensions/mv2/manifest/)
5. 搜狗浏览器插件路径: `%AppData%\SogouExplorer\Extension`{: .filepath}, Edge 插件路径: `%LocalAppData%\Microsoft\Edge\User Data\Default\Extensions`{: .filepath}
