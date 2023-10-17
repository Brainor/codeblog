---
tags: javascript
date: 2023-10-16 12:41:00 +8000
---

## 参考
[MDN 文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference)

## 语句, 对象
### [import](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/import) 与 [export](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/export)
ECMAScript模块使用`import`来导入模块，使用`export`来导出模块。
静态的import语句用于导入由另一个模块导出的绑定。  
export语句用于从模块中导出实时绑定的函数、对象或原始值，以便其他程序可以通过 import 语句使用它们。
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

### [require](https://nodejs.org/api/modules.html#requireid)与[module.exports](https://nodejs.org/api/modules.html#moduleexports)
CommonJS使用`require`来导入模块，使用`module.exports`或`exports`来导出模块。
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
Promise对象用于表示一个异步操作的最终完成 (或失败), 及其结果值.  
`Promise((resolve, reject)=>{...}).then(resolve, reject).catch(reject)`

### [IIFE](https://developer.mozilla.org/zh-CN/docs/Glossary/IIFE)
立即调用的函数表达式(Immediately Invoked Function Expression)是一个在定义时就会立即执行的  JavaScript 函数.  
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
webpack运行在Node.js环境中
