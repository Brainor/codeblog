---
tags: javascript
date: 2023-10-16 12:41:00 +8000
---

## 参考
[MDN 文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference)

## 语句, 对象
### [import](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/import) 与 [export](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/export)
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

### [Promise](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)
Promise对象用于表示一个异步操作的最终完成 (或失败), 及其结果值.  
`Promise((resolve, reject)=>{...}).then(resolve, reject).catch(reject)`