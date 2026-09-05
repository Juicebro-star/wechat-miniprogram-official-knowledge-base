---
title: "引用外部样式"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/external-classes.html"
source_route: "/miniprogram/dev/framework/component-framework/external-classes.html"
raw_html: "raw-html/component-framework/external-classes.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/external-classes.html](https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/external-classes.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/component-framework/external-classes.html`

# 引用外部样式

在组件样式隔离的前提下，有些组件希望接受组件使用者传入的样式类。

## 外部样式类

自基础库版本 [1.9.90](../compatibility) 起，可以用 `externalClasses` 定义若干个外部样式类。

这个特性可以用于实现类似于 `view` 组件的 `hover-class` 属性：页面可以提供一个样式类，赋予 `view` 的 `hover-class`，这个样式类本身写在页面的 WXSS 中而非 `view` 组件中。

```
/* foo-component.js */
export default Component({
  externalClasses: ['my-class'],
})
```

```
<!-- foo-component.wxml -->
<foo-component class="my-class">这段文本的颜色受组件引用者的 class 影响</foo-component>
```

这样，组件的使用者可以指定这个样式类对应的 class ，就像使用普通属性一样。在基础库版本 [2.7.1](../compatibility) 之后，可以指定多个对应的 class 。

```
/* 页面的 WXSS */
.red-text {
  color: red;
}
.large-text {
  font-size: 1.5em;
}
```

```
<!-- 页面的 WXML -->
<foo-component my-class="red-text" />
<foo-component my-class="red-text large-text" />
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/RG7Khvmo81bb)

注意：在同一个节点上使用普通样式类和外部样式类时，两个类的优先级是未定义的。可以借助选择器优先级机制来控制两个类的优先级。

## 多层嵌套传递外部样式类

外部样式类可以多层嵌套传递。例如，定义一个 `bar-component` 来引用上面的 `foo-component`：

```
/* bar-component.js */
export default Component()
  .externalClasses(['bar-class'])
  .register()
```

```
<!-- bar-component.wxml -->
<foo-component my-class="bar-class" />
```

页面在引用 `bar-component` 时：

```
<!-- 页面的 WXML -->
<bar-component bar-class="red-text" />
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/T97IgvmR81bu)

此时 `.red-text` 会从页面经过 `bar-component` 最终传递到 `foo-component` 中。

## 直接引用父组件的样式

即使启用了样式隔离 `isolated`，组件仍然可以通过一些特殊语法来使用组件所在页面的样式类或父组件的样式类。

需要注意的是，这会增加组件间的不良耦合，优先推荐使用 [外部样式类](#外部样式类) 来代替。

例如，如果在页面 WXSS 中定义了：

```
.blue-text {
  color: blue;
}
```

在这个组件中可以使用 `~` 来引用页面中这个类的样式：

```
<view class="~blue-text"> 这段文本是蓝色的 </view>
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/Lx7qjvmb81bz)

其中的 `~` 表示引用页面中对应名称的样式类。

另外，可以使用 `^` 表示引用父组件中对应名称的样式类；也可以连续使用多个 `^` 来引用祖先组件中的。

