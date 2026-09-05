---
title: "slot 模式"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/slot.html"
source_route: "/miniprogram/dev/framework/component-framework/slot.html"
raw_html: "raw-html/component-framework/slot.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/slot.html](https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/slot.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/component-framework/slot.html`

# slot 模式

每个组件实例都拥有独立的 [Shadow 树](../custom-component/component-system#shadow-树)，代表组件自身模板中的节点结构。当组件的使用者在组件标签内放置子节点时，这些子节点会被视为 slot 内容，放置到 `<slot>` 的位置上。换句话说，`<slot>` 是一个占位符，代表 slot 内容所在的位置。

自定义组件使用 `<slot>` 时，slot 有三种工作模式可选。每个组件都可以任选其一。

- **单一 slot** 是默认情况，仅支持一个 `<slot>` 节点；
- **多 slot** 支持多个 `<slot>`，每个 slot 需要有一个名字 `<slot name="xxx">`；
- **动态 slot** 支持在 `wx:for` 内部使用 `<slot>`，同时可以向 slot 内容节点树传递数据。

## 单一 slot

默认情况下激活的是单一 slot 模式。这种模式具有最优的性能。

在这个模式下，自定义组件 WXML 中只能包含一个 `<slot>` 节点。例如：

```
<!-- custom-component.wxml -->
<view>
  <slot />
</view>
```

使用这个自定义组件时，传入内容节点：

```
<custom-component>
  <view> content </view>
</custom-component>
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/dK7AYvmn81bU)

`<view> content </view>` 最终会被放置到 `<slot>` 的位置上。

如果在自定义组件 WXML 内写了多个 `<slot>` 节点，只有第一个生效。

## 多 slot

在组件 options 中声明 `multipleSlots: true` 可激活多 slot 模式。

```
// Definition 形式的 custom-component.json
export default Component({
  options: {
    multipleSlots: true,
  },
})
```

```
// Chaining 形式的 custom-component.json
export default Component()
  .options({ multipleSlots: true })
  .register()
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/mj8rrvmi8Tbe)

多 slot 模式允许自定义组件 WXML 中包含多个 `<slot>` 节点。每个 `<slot>` 都必须有名字 `name`，其值必须是一个唯一的字符串。例如：

```
<!-- custom-component.wxml -->
<view class="foo-wrapper">
  <slot name="foo" />
</view>
<view class="bar-wrapper">
  <slot name="bar" />
</view>
```

在使用这个自定义组件时，需要为每个 slot 内容节点指定它所在的 `slot` 名字，例如：

```
<custom-component>
  <!-- 这个 <view> 会放入 <slot name="foo"> 中 -->
  <view slot="foo"> foo content </view>

  <!-- 可以用 block 来避免额外生成一个 <view> -->
  <block slot="bar"> bar content </block>
</custom-component>
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/jgEt1Amj8zby)

## 静态 slot 的 slot 内容生命周期

单一 slot 和多 slot 模式统一称为静态 slot 模式。

在静态 slot 下，即使自定义组件没有 `<slot>` 节点，slot 内容节点依然会被创建并触发 `attached` `detached` 等生命周期。例如，如果在自定义组件里面这么写：

```
<!-- custom-component.wxml -->
<view wx:if="{{ cond }}">
  <slot />
</view>
```

而在页面内这样写：

```
<custom-component>
  <view> content </view>
  <another-custom-component />
</custom-component>
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/gF80svmW8Tbv)

如果 `cond` 为假，`<slot>` 节点就不存在，但是：

- 即使 `<slot>` 节点不存在，`<view> content </view>` 和 `<another-custom-component>` 仍然会被创建，`<another-custom-component>` 的 `attached` 生命周期依然会被触发；
- 如果后续 `cond` 变成真，`<slot>` 节点会被创建，但是 `<view> content </view>` 和 `<another-custom-component>` 并不会被重新创建，`<another-custom-component>` 的 `attached` 生命周期也不会再次触发。

如果想要 slot 内容与 `<slot>` 节点一起创建或销毁，可以使用动态 slot 模式。

## 动态 slot

动态 slot 模式允许自定义组件使用若干个 `<slot>` 节点。

在组件 options 中声明 `dynamicSlots: true` 可激活动态 slot 模式。

```
// Definition 形式的 custom-component.json
export default Component({
  options: {
    dynamicSlots: true,
  },
})
```

```
// Chaining 形式的 custom-component.json
export default Component()
  .options({ dynamicSlots: true })
  .register()
```

在使用动态 slot 时，每个 `<slot>` 拥有 slot 内容节点树的一份拷贝（这份拷贝会随着 `<slot>` 一同创建和销毁）。例如对于组件：

```
<!-- custom-component.wxml -->
<view class="foo">
  <slot />
</view>
<view class="bar">
  <slot />
</view>
```

在页面内这样写：

```
<custom-component>
  <view> content </view>
</custom-component>
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/qW8xmvmD8TbH)

将会最终出现两个 `<view> content </view>`，分别位于 `<view class="foo">` 和 `<view class="bar">` 内部。

同时，`<slot>` 节点可以传递数据字段，让 slot 内容节点树使用。这使得动态 slot 非常适合 `<slot>` 节点出现在 `wx:for` 列表内部的情况。例如对于组件：

```
<!-- custom-component.wxml -->
<view wx:for="{{ list }}">
  <slot list-index="{{ index }}" item="{{ item }}" />
</view>
```

通过 `slot:` 语法，在页面内可以接收到 `list-index` 和 `item`：

```
<custom-component>
  <view slot:item> {{ item }} </view>
</custom-component>
```

使用 `slot:` 接收数据时，可以用 `=` 来指定一个别名。此外，还可以使用 `<block>` 来避免生成额外的节点：

```
<custom-component>
  <block slot:listIndex="i"> {{ i }} </block>
  <block slot:item> {{ item }} </block>
</custom-component>
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/tmRDRAmh84bH)

动态 slot 模式同样支持类似多 slot 的 name 属性，不过一般并不常用。

