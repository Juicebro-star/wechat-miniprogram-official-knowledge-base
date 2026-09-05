---
title: "模块化"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/module.html"
source_route: "/miniprogram/dev/framework/app-service/module.html"
raw_html: "raw-html/app-service/module.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/module.html](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/module.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/app-service/module.html`

# 模块化

## 全局模块

可以将一些公共的代码抽离成为一个单独的 js 文件，作为一个模块。模块只有通过 [`module.exports`](https://developers.weixin.qq.com/miniprogram/dev/reference/api/module) 或者 `exports` 才能对外暴露接口。

注意：

- `exports` 是 [`module.exports`](https://developers.weixin.qq.com/miniprogram/dev/reference/api/module) 的一个引用，因此在模块里边随意更改 `exports` 的指向会造成未知的错误。所以不建议同时使用两者，除非很清楚它们之间的关系。
- 在小程序中直接引入 `node_modules` 并不会生效。若需使用，请参考 [构建 npm](https://developers.weixin.qq.com/miniprogram/dev/devtools/npm) 功能。

```
// common.js
function sayHello(name) {
  console.log(`Hello ${name} !`)
}
function sayGoodbye(name) {
  console.log(`Goodbye ${name} !`)
}

exports.sayHello = sayHello
exports.sayGoodbye = sayGoodbye
```

在需要使用这些模块的文件中，使用 `require` 将公共代码引入。

```
var common = require('common.js')
Page({
  helloMINA: function() {
    common.sayHello('MINA')
  },
  goodbyeMINA: function() {
    common.sayGoodbye('MINA')
  }
})
```

## 文件作用域

在 JavaScript 文件中声明的变量和函数只在该文件中有效；不同的文件中可以声明相同名字的变量和函数，不会互相影响。

```
// a.js
var localValue = 'a'
```

```
// b.js
var localValue = 'b'
```

上述两个 `localValue` 是不同的两个变量。

## 全局变量

若想要一个变量可以被全局使用，可以用 `exports` 将它导出。

```
// common.js
exports.globalValue = 'c'
```

```
var common = require('common.js')

common.globalValue // 'c'
```

