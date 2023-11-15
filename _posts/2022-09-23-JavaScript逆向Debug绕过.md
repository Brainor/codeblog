---
tag: javascript hack
img_path: /assets/files/
categories: coding
---
源地址: [进击的Coder](https://mp.weixin.qq.com/s/e5vyAQbVyBONBimsHPvaaw)

在做爬虫的时候，我们经常会在代码里面遇见 `debugger` 这么一个关键字。`debugger` 是 JavaScript 中定义的一个专门用于断点调试的关键字，只要遇到它，JavaScript 的执行便会在此处中断，进入调试模式。

有了 `debugger` 这个关键字，我们可以非常方便地对 JavaScript 代码进行调试，比如使用 JavaScript Hook 时，我们可以加入 `debugger` 关键字，使其在关键的位置停下来，以便查找逆向突破口。

但有时候，`debugger` 会被网站开发者利用，使其成为阻挠我们正常调试的拦路虎。

本节我们就来介绍一个案例，来绕过无限 Debug。

## 1. 案例介绍
我们先看一个案例，网址是：[https://antispider8.scrape.center/](https://antispider8.scrape.center/) ，打开这个网站，正常操作和之前的网站没有什么不同。但是，一旦我们打开开发者工具，就发现它立即进入了断点模式，如图所示。

<figure>
    <img src='2022-09-23-JavaScript逆向Debug绕过.1.png' alt='missing' />
    <figcaption style='text-align:center'>进入断点模式</figcaption>
</figure>

我们并没有设置任何断点，也没有执行任何额外的脚本，它就直接进入了断点模式。这时候我们可以点击 Resume script execution （恢复脚本执行）按钮，尝试跳过这个断点继续执行，如图所示。

<figure>
    <img src='2022-09-23-JavaScript逆向Debug绕过.2.png' alt='missing' />
    <figcaption style='text-align:center'>尝试跳过断点</figcaption>
</figure>

然而不管我们按多少次，它仍然一次次地进入断点模式，无限循环下去，我们可以称这样的情况为无限 Debugger。

这怎么办呢？似乎无法正常打断点调试了，有什么解决办法吗？

办法当然是有的，本节我们就来总结一下无限 Debugger 的应对方案。

## 2. 实现原理
我们首先要做的是找到无限 Debugger 的源头。在 Sources 面板中可以看到，`debugger` 关键字出现在了一个 JavaScript 文件里，这时候点击左下角的格式化按钮，如图所示。

<figure>
    <img src='2022-09-23-JavaScript逆向Debug绕过.3.png' alt='missing' />
    <figcaption style='text-align:center'>点击 Sources 面板中的格式化按钮</figcaption>
</figure>

这里通过 `setInterval` 循环，每秒执行 1 次 `debugger` 语句，如图所示。

<figure>
  <img src="2022-09-23-JavaScript逆向Debug绕过.4.png" alt="missing" />
  <figcaption style='text-align:center'>每秒执行 1 次 debugger 语句</figcaption>
</figure>

当然还有很多类似的实现，比如无限 `for` 循环、无限 `while` 循环、无限递归调用等，它们都是可以实现这样的效果的，原理大同小异。

了解了原理，下面我们就对症下药吧！

## 3. 禁用断点
因为 `debugger` 其实就是对应的一个断点，它相当于用代码显式地声明了一个断点，要解除它，我们只需要禁用这个断点就好了。

首先，我们可以禁用所有的断点。全局禁用开关位于 Sources 面板的右上角，叫作 Deactivate breakpoints，如图所示。

<figure>
  <img src="2022-09-23-JavaScript逆向Debug绕过.5.png" alt="missing" />
  <figcaption style='text-align:center'>全局禁用开关</figcaption>
</figure>

点击一下它，这时候就会发现所有的断点变成了灰色，如图所示。

<figure>
  <img src="2022-09-23-JavaScript逆向Debug绕过.6.png" alt="missing" />
  <figcaption style='text-align:center'>禁用所有的断点</figcaption>
</figure>

这时候我们再重新点击一下 Resume script execution 按钮，跳过当前断点，页面就不会再进入到无限 Debugger 的状态了。

但是这种全局禁用其实并不是一个好的方案，因为禁用之后我们也无法在其他位置增加断点进行调试了，所有的断点都失效了。

这时候，我们可以选择禁用局部断点。取消刚才的 Deactivate breakpoints 模式，页面会重新进入无限 Debugger 模式，我们尝试使用另一种方法来跳过这个无限 Debugger。

我们可能会想着去掉 Breakpoints 里勾选的断点，心想这样不就禁用了吗？大家尝试一下取消勾选，如图所示。

<figure>
  <img src="2022-09-23-JavaScript逆向Debug绕过.7.png" alt="missing" />
  <figcaption style='text-align:center'>取消勾选</figcaption>
</figure>

然而，取消之后再继续点击 Resume 按钮，它依然不断地停在有 `debugger` 关键字的地方，并没有什么效果。

其实，Breakpoints 只代表了我们手动添加的断点，对于 `debugger` 关键字声明的断点，在这里直接取消是没有用的。

那这种情况下还有什么办法吗？

有的。我们可以先将当前 Breakpoints 里面的断点删除，然后在 debugger 语句所在的行的行号上单击鼠标右键，这里会出现一个下拉菜单，如图所示。

<figure>
  <img src="2022-09-23-JavaScript逆向Debug绕过.8.png" alt="missing" />
  <figcaption style='text-align:center'>在行号上单击鼠标右键</figcaption>
</figure>

这里会有一个叫作 Never pause here 的选项，意思是从不在此处暂停，我们选择这个选项，于是页面变成如图所示的样子。

<figure>
  <img src="2022-09-23-JavaScript逆向Debug绕过.9.png" alt="missing" />
  <figcaption style='text-align:center'>点击 Never pause here 选项后的页面</figcaption>
</figure>

当前断点显示为橙色，并且断点前面多了一个 `?` 符号，同时 Breakpoints 也出现了刚才添加的断点位置。这时再次点击 Resume 按钮，就可以发现我们不会再进入无限 Debugger 模式了。

当然我们也可以选择另外一个选项 Add conditional breakpoint，如图所示。

<figure>
  <img src="2022-09-23-JavaScript逆向Debug绕过.10.png" alt="missing" />
  <figcaption style='text-align:center'>Add conditional breakpoint 选项</figcaption>
</figure>

这个模式更加高级，我们可以设置进入断点的条件，比如在调试过程中，期望某个变量的值大于某个具体值的时候才停下来。但在本案例中，由于这里是无限循环，所以我们没有什么具体的变量可以作为判定依据，因此可以直接写一个简单的表达式来控制。

点击 Add conditional breakpoint 选项，直接填入 false 即可，如图所示。

<figure>
  <img src="2022-09-23-JavaScript逆向Debug绕过.11.png" alt="missing" />
  <figcaption style='text-align:center'>设置 Conditional breakpoint 为 false</figcaption>
</figure>

设定为 false，其效果就和选择了 Never pause here 是一样的，重新点击 Resume 也不会进入无限 Debbugger 循环了。

## 4. 替换文件
前文我们介绍过 Overrides 面板的用法，利用它我们可以将远程的 JavaScript 文件替换成本地的 JavaScript 文件，这里我们依然可以使用这个方法来对文件进行替换，替换成什么呢？

很简单，我们只需要在新的文件里面把 `debugger` 这个关键字删除。

我们将当前的 JavaScript 文件复制到文本编辑器中，删除或者直接注释掉 `debugger` 这个关键字，修改如下：
```js
setInterval((function() {
 // debugger; // 可以直接删除此行或者注释此行
 console.log("debugger")
}), 1000)
```
打开 Sources 面板下的 Overrides 面板，将修改后的完整 JavaScript 文件复制进去，修改的内容如图所示：

<figure>
  <img src="2022-09-23-JavaScript逆向Debug绕过.12.png" alt="missing" />
  <figcaption style='text-align:center'>替换 JavaScript 文件</figcaption>
</figure>

替换完成之后，我们重新刷新网页，这时候就发现不会进入无限 Debugger 模式了。

另外我们还可以使用 Charles、Fiddler 等抓包工具进行替换，也可以使用浏览器插件 ReRes 等进行替换，也可以使用 Playwright 等工具使用 Request Interception 进行替换，达成的效果是一致的，原理都是将在线加载的 JavaScript 文件进行替换，最终消除无限 Debugger。

## 5. 总结
本节讲解了无限 Debugger 的绕过方案，包括禁用全局断点、条件断点、替换原始文件等，从这些操作中我们也可以学习到一些 JavaScript 逆向的基本思路，建议好好掌握本内容。

本文章来源于《Python3网络爬虫开发实战（第二版）》