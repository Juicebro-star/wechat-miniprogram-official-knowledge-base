---
title: "WXSS"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxss.html"
source_route: "/miniprogram/dev/framework/view/wxss.html"
raw_html: "raw-html/view/wxss.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxss.html](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxss.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/view/wxss.html`

# WXSS

WXSS（WeiXin Style Sheets）是一套样式语言，用于描述 WXML 的组件样式。

为了适应广大的前端开发者，WXSS 具有 CSS 大部分特性。以下是一些常见的 WXSS 语法。

## 样式规则定义

与 CSS 一致，可以通过选择器指定节点的样式。

```
/* page.wxss */
/* 指定 class 含有 important-text 的节点的样式 */
.important-text {
  font-size: 1.2em;
  color: red;
}
```

上面的样式规则可以被应用到对应的 WXML 中。

```
<!-- page.wxml -->
<view class="important-text"> 重要信息 </view>
```

## 合理使用选择器

WXSS 支持绝大多数 CSS 选择器，但有个别例外：属性名选择器 `[...]` 不会生效；不支持带参数的伪类和伪元素选择器。

从最佳实践上看，在编写样式表时只推荐使用 class 选择器，因为这样往往可以让代码更加清晰。下面是一个简单的例子：

```
.hint {
  padding: 5px;
}
.important-text {
  font-size: 1.2em;
  color: red;
}
.minor-text {
  font-size: 0.8em;
  color: #666;
}
```

```
<!-- page.wxml -->
<view class="hint important-text"> 重要信息 </view>
<view class="hint"> 信息 </view>
<view class="hint minor-text"> 附注 </view>
```

## 内联样式

样式规则内容也可以直接写在 WXML 内的节点 `style` 中，例如：

```
<!-- 可以将样式写在 style="..." 中，不过实践中要慎用 -->
<view style="font-size: 1.2em; color: red;"> 重要信息 </view>
```

从最佳实践上看，`style` 中不应放置很长的样式规则，否则会大大降低代码的可读性。通常，只有要用到数据绑定时，才使用这种写法，例如：

```
<!-- page.wxml -->
<view style="color: {{color}};"> {{colorName}} </view>
```

```
// page.js
Page({
  data: {
    color: 'yellow',
    colorName: '黄色',
  },
})
```

## 样式导入

类似于 CSS，可以使用 `@import` 导入其他 WXSS 文件。这是一种简单的跨页面样式代码共享的方式。

```
/** common.wxss **/
.hint {
  padding: 5px;
}
```

```
/** page-a.wxss **/
@import "common.wxss"; /* 相当于将 common.wxss 的内容插在这里 */
.minor-text {
  font-size: 0.8em;
  color: #666;
}
```

```
/** page-b.wxss **/
@import "common.wxss"; /* 相当于将 common.wxss 的内容插在这里 */
.important-text {
  font-size: 1.2em;
  color: red;
}
```

## 全局样式

`app.wxss` 文件中的样式为全局样式，一般情况下会作用于所有页面（页面也可以通过配置禁止 `app.wxss` 对它生效）。

从最佳实践上看，`app.wxss` 只适合放入绝大多数页面都必须要用到的样式。改用 `@import` 样式导入的方式往往更为灵活。

## 基于屏幕宽度比例的长度单位

在实践中，有些页面内容的尺寸需要根据屏幕尺寸来确定。

在这种情况下，可以使用 CSS 标准的 `vw` 长度单位。`1vw` 等于页面总宽度的百分之一。例如，页面宽度为 `375px` 时，`1vw` 就相当于 `3.75px`。

由于历史原因，WXSS 也支持一个类似的长度单位 `rpx`，用于表达页面总宽度的 `1 / 750`。例如，页面宽度为 `375px` 时，`1rpx` 就相当于 `0.5px`。不过，`rpx` 的计算规则有时会对转换后的数值进行取整，从而带来一些精度问题；想避免这样的问题，可以考虑开启 [convertRpxToVw](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app) 选项。此外，由于现在 CSS 标准的 `vw` 单位已经得到广泛支持，所以还是优先推荐使用 `vw`。

注意：考虑到对手机、PC 等大屏幕设备的支持， **不建议滥用 `vw` `rpx` 等基于屏幕宽度比例的单位** ；如果不可避免，请联合使用 Media Query 等响应式布局方式，详情参考 [响应显示区域变化](resizable) 。

## 渲染引擎对 WXSS 的影响

请注意，渲染引擎会对 WXSS 语法有额外的限制：

- 如果在使用默认的 WebView 渲染引擎，由于不同操作系统自带的 WebView 引擎也不尽相同，请注意样式表兼容性也会受到不同版本操作系统的影响；
- 如果在使用 Skyline 渲染引擎，请额外参考它的 [相关说明](../runtime/skyline/wxss) 。

