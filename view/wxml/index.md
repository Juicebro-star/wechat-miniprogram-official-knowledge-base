---
title: "WXML"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/"
source_route: "/miniprogram/dev/framework/view/wxml/"
raw_html: "raw-html/view/wxml/index.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/view/wxml/index.html`

# WXML

WXML（WeiXin Markup Language），结合 [基础组件](index)、[事件系统](event)，可以构建出页面的结构。

要完整了解 WXML 语法，请参考 [WXML 语法参考](https://developers.weixin.qq.com/miniprogram/dev/reference/wxml/)。

以下是一些常用的 WXML 语法。

## 数据绑定

```
<!-- page.wxml -->
<view> {{message}} </view>
```

```
// page.js
Page({
  data: {
    message: 'Hello MINA!'
  }
})
```

## 列表渲染

```
<!-- page.wxml -->
<view wx:for="{{array}}"> {{item}} </view>
```

```
// page.js
Page({
  data: {
    array: [1, 2, 3, 4, 5]
  }
})
```

## 条件渲染

```
<!-- page.wxml-->
<view wx:if="{{view == 'WEBVIEW'}}"> WEBVIEW </view>
<view wx:elif="{{view == 'APP'}}"> APP </view>
<view wx:elif="{{view == 'MINA'}}"> MINA </view>
<view wx:else> UNKNOWN </view>
```

```
// page.js
Page({
  data: {
    view: 'MINA'
  }
})
```

## 模板

```
<!-- page.wxml -->
<template name="staffName">
  <view>
    FirstName: {{firstName}}, LastName: {{lastName}}
  </view>
</template>

<template is="staffName" data="{{...staffA}}"></template>
<template is="staffName" data="{{...staffB}}"></template>
<template is="staffName" data="{{...staffC}}"></template>
```

```
// page.js
Page({
  data: {
    staffA: {firstName: 'Hulk', lastName: 'Hu'},
    staffB: {firstName: 'Shang', lastName: 'You'},
    staffC: {firstName: 'Gideon', lastName: 'Lin'}
  }
})
```

