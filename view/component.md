---
title: "基础组件"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/view/component.html"
source_route: "/miniprogram/dev/framework/view/component.html"
raw_html: "raw-html/view/component.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/view/component.html](https://developers.weixin.qq.com/miniprogram/dev/framework/view/component.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/view/component.html`

# 基础组件

## 组件标签

WXML 中包含有 `<...>` 这样的标签。其中的很多都对应于 **组件** ，例如 `<view>` 就对应于 [view 组件](https://developers.weixin.qq.com/miniprogram/dev/component/view.html) ，`<image>` 就对应于 [image 组件](https://developers.weixin.qq.com/miniprogram/dev/component/image.html)。

框架内置了很多这样的基础组件，可以在 [组件文档](miniprogram/dev/component/) 中查询。此外，也可以自己编写一些 [自定义组件](../component-framework/index) 。

## 组件属性

在组件标签上可以填写组件属性对应的属性值，其中可以使用数据绑定。不同属性会接受不同类型的值。

`number` 数值和 `string` 字符串是比较常见的属性类型，例如 image 组件的 `src` 属性就是 `string` 类型的：

```
<!-- 使用静态字符串 -->
<image src="https://res.wx.qq.com/wxdoc/dist/assets/img/0.4cb08bb4.jpg" />

<!-- 使用数据绑定 -->
<image src="{{ imageUrl }}" />
```

`boolean` 类型的属性接受 `true` 或者 `false` 值。请注意，除非使用数据绑定，否则它都会被视为 `true`，以 view 组件的 `hidden` 属性为例：

```
<!-- 这样写视为将 hidden 设置为 true -->
<view hidden> 这些文字会被隐藏 </view>

<!-- 这样写也视为将 hidden 设置为 true -->
<view hidden="any-value"> 这些文字也会被隐藏 </view>

<!-- 如果需要设置为 false，就要用数据绑定的写法 -->
<view hidden="{{ false }}"> 这些文字会显示出来 </view>
```

其他的属性类型，如 `array` 和 `object` ，则一定要使用数据绑定的写法。

## 基础组件公共属性

所有基础组件都具有 [无障碍访问](https://developers.weixin.qq.com/miniprogram/dev/component/aria-component.html) 相关属性。

此外，基础组件还都有以下属性。

| 属性 | 类型 | 默认值 | 必填 | 说明 | 最低版本 |
| --- | --- | --- | --- | --- | --- |
| hidden | boolean | false | 否 | 是否隐藏这个节点，相当于 WXSS `display: none` 的效果 | [1.0.0](../compatibility) |

