---
title: "简易双向绑定"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/view/two-way-bindings.html"
source_route: "/miniprogram/dev/framework/view/two-way-bindings.html"
raw_html: "raw-html/view/two-way-bindings.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/view/two-way-bindings.html](https://developers.weixin.qq.com/miniprogram/dev/framework/view/two-way-bindings.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/view/two-way-bindings.html`

# 简易双向绑定

> 基础库 2.9.3 开始支持，低版本需做[兼容处理](../compatibility)。

## 双向绑定语法

在 WXML 中，普通的属性的绑定是单向的。例如：

```
<input value="{{ value }}" />
```

如果使用 `this.setData({ value: 'leaf' })` 来更新 `value`，`this.data.value` 和输入框的中显示的值都会被更新为 `leaf`；但如果用户修改了输入框里的值，却不会同时改变 `this.data.value`。

如果需要在用户输入的同时改变 `this.data.value`，需要借助简易双向绑定机制。此时，可以在对应项目之前加入 `model:` 前缀：

```
<input model:value="{{ value }}" />
```

这样，如果输入框的值被改变了，`this.data.value` 也会同时改变。同时，WXML 中所有绑定了 `value` 的位置也会被一同更新，[数据监听器](../observer) 也会被正常触发。

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/8jXvobmV7vcj "在开发者工具中预览效果")

## 左值表达式

用于双向绑定的表达式有一定限制。对于 glass-easel 组件框架，绑定的表达式必须是一个 **左值表达式**，即只能包含以下几种形式及其组合：

- data 字段名，例如 `foo`；
- WXML `let:` 生成的临时变量、`wx:for` 生成的临时变量（`wx:for-item`），前提是生成它们的表达式也是左值表达式；
- 合法左值表达式的子字段，例如 `foo.bar`；
- 合法左值表达式的动态字段名子字段（数组索引项），例如 `foo['bar']`，其中方括号中的表达式可以是任意表达式。

以下这些例子是合法的：

```
<!-- 普通字段 -->
<input model:value="{{ foo }}" />

<!-- let 生成的临时变量 -->
<block let:bar="{{ foo }}"> <!-- let 表达式也必须是左值表达式 -->
  <input model:value="{{ bar }}" />
</block>

<!-- wx:for 生成的临时变量 -->
<view wx:for="{{ list }}"> <!-- wx:for 表达式也必须是左值表达式 -->
  <input model:value="{{ item }}" />
</view>

<!-- 合法左值表达式的子字段 -->
<input model:value="{{ foo.bar }}" />

<!-- 合法左值表达式的动态字段名子字段 -->
<input model:value="{{ foo['bar'] }}" />
```

以下这些例子是非法的：

```
<!-- 绑定必须是纯粹的表达式，非法！ -->
<input model:value="值为 {{ foo }}" />

<!-- 用了不合法的运算符，非法！ -->
<input model:value="{{ a + b }}" />

<!-- 来源不是数据字段，非法！ -->
<wxs module="foo">
  exports.bar = 1;
</wxs>
<input model:value="{{ foo.bar }}" />
```

对于 exparser 组件框架，限制更为严格：绑定的表达式只能是单个 data 字段变量。

```
<!-- exparser 不支持复杂表达式 -->
<input model:value="{{ foo.bar }}" />
```

## 在自定义组件中传递双向绑定

双向绑定同样可以使用在自定义组件上。例如：

```
// custom-component.js
export default Component({
  properties: {
    myValue: String,
  }
})
```

```
<!-- custom-component.wxml -->
<input model:value="{{ myValue }}" />
```

这个组件将自身的 `myValue` 属性双向绑定到了组件内输入框的 `value` 属性上。这样，如果页面这样使用这个组件：

```
<custom-component model:my-value="{{ pageValue }}" />
```

当输入框的值变更时，自定义组件的 `myValue` 属性会同时变更，这样，页面的 `this.data.pageValue` 也会同时变更，页面 WXML 中所有绑定了 `pageValue` 的位置也会被一同更新。

## 在自定义组件中触发双向绑定更新

自定义组件还可以自己触发双向绑定更新，做法就是：使用 setData 设置自身的属性。例如：

```
// custom-component.js
Component({
  properties: {
    myValue: String,
  },
  methods: {
    update: function() {
      // 更新 myValue
      this.setData({
        myValue: 'leaf'
      })
    },
  },
})
```

如果页面这样使用这个组件：

```
<custom-component model:my-value="{{ pageValue }}" />
```

当组件使用 `setData` 更新 `myValue` 时，页面的 `this.data.pageValue` 也会同时变更，页面 WXML 中所有绑定了 `pageValue` 的位置也会被一同更新。

