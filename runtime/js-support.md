---
title: "JavaScript 支持情况"
type: source_document
layer: L4
owner: external
claim_type: fact
status: archived
domain: mini_program
project: wechat_miniprogram_docs
source_type: official_web
visibility: public
retrieved: "2026-09-05"
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/runtime/js-support.html"
source_route: "/miniprogram/dev/framework/runtime/js-support.html"
raw_html: "raw-html/runtime/js-support.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/runtime/js-support.html](https://developers.weixin.qq.com/miniprogram/dev/framework/runtime/js-support.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/runtime/js-support.html`

# JavaScript 支持情况

## 运行限制

基于安全考虑，小程序中不支持动态执行 JS 代码，即：

- 不支持使用 `eval` 执行 JS 代码
- 不支持使用 `new Function` 创建函数
  - `new Function('return this')` 除外

## 标准 ECMAScript 支持

小程序的 JS [执行环境](env) 在不同平台上的执行环境存在差异，因此导致不同平台对 ECMAScript 标准的支持存在差异。

小程序基础库为了尽量抹平这些差异，内置了一份 [`core-js` Polyfill](https://github.com/zloirock/core-js)。`core-js` 可以将平台环境缺失的标准 API 补齐。

需要注意的是，平台对 ECMAScript 语法的支持差异无法抹平，当你需要使用一些高级语法时，如 [`async/await`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/async_function) 时，则需要借助 [代码转换工具](https://developers.weixin.qq.com/miniprogram/dev/devtools/codecompile#es6-%E8%BD%AC-es5) 来支持这些语法。

关于如何判断当前环境需要哪些 Polyfill、代码转换目标，可以参考 [微信小程序 JS 执行环境兼容信息](https://github.com/wechat-miniprogram/miniprogram-compat) 开源项目。

## 与标准的差异

### Promise 时序差异

由于 iOS JavaScriptCore 的限制，iOS 15 及以下的 `Promise` 是一个使用 `setTimeout` 模拟的 Polyfill。这意味着 `Promise` 触发的任务为普通任务，而非微任务，进而导致 **在 iOS 15 及以下的 `Promise` 时序会和标准存在差异**。

iOS 16 及以上不存在差异。

```
var arr = []

setTimeout(() => arr.push(6), 0)
arr.push(1)
const p = new Promise(resolve => {
  arr.push(2)
  resolve()
})
arr.push(3)
p.then(() => arr.push(5))
arr.push(4)
setTimeout(() => arr.push(7), 0)

setTimeout(() => {
  // 应该输出 [1,2,3,4,5,6,7]
  // 在 iOS15 小程序环境，这里会输出 [1,2,3,4,6,5,7]
  console.log(arr)
}, 1000)
```

关于普通任务和微任务的区别可以查看[这篇文章](https://developer.mozilla.org/zh-CN/docs/Web/API/HTML_DOM_API/Microtask_guide/In_depth)

