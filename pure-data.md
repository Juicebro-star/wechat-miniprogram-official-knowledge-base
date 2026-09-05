---
title: "纯数据字段"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/pure-data.html"
source_route: "/miniprogram/dev/framework/pure-data.html"
raw_html: "raw-html/pure-data.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/pure-data.html](https://developers.weixin.qq.com/miniprogram/dev/framework/pure-data.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/pure-data.html`

# 纯数据字段

纯数据字段是一些 WXML 模板中没用到的 data 字段。从小程序基础库版本 [2.8.2](compatibility) 开始支持。

在 exparser 组件框架中，使用纯数据字段有助于提升整体性能。在 glass-easel 组件框架中，使用纯数据字段对整体性能的影响很小。

如果使用 Chaining 形式，不用于 WXML 模板的数据应优先考虑使用 `init` 函数内的局部变量代替（这样也就用不上纯数据字段了）。

```
export default Component()
  .data(() => ({
    // data 中包含的数据应当是用于 WXML 模板的
    dataUsedInTemplate: true,
  }))
  .init(function () {
    // 不用于 WXML 模板的数据就写成局部变量
    const dataNotUsedInTemplate = {}
  })
  .register()
```

## 组件数据中的纯数据字段

指定纯数据字段的方法是在 `Component` 构造器的 `options` 定义段中指定 `pureDataPattern` 为一个正则表达式，字段名符合这个正则表达式的字段将成为纯数据字段。

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/DKWiBXmb7jaB "在开发者工具中预览效果")

**代码示例：**

```
export default Component({
  options: {
    pureDataPattern: /^_/ // 指定所有 _ 开头的数据字段为纯数据字段
  },
  data: {
    a: true, // 普通数据字段
    _b: true, // 纯数据字段
  },
  methods: {
    myMethod() {
      this.data._b // 纯数据字段可以在 this.data 中获取
      this.setData({
        c: true,
        _d: true, // 纯数据字段也可以被 setData 设置
      })
    },
  },
})
```

上述组件中的纯数据字段不会被应用到 WXML 上：

```
<view wx:if="{{a}}"> 这行会被展示 </view>
<view wx:if="{{_b}}"> 这行不会被展示 </view>
```

虽然不能用于 WXML，但纯数据字段可以使用 [数据监听器](observer) 来监听变化。

## 组件属性中的纯数据字段

属性也可以被指定为纯数据字段（遵循 `pureDataPattern` 的正则表达式）。属性中的纯数据字段可以像普通属性一样接收外部传入的属性值，但不能将它直接用于组件自身的 WXML 中。

注意：属性中的纯数据字段的属性 observer 永远不会触发！如果想要监听属性值变化，使用 [数据监听器](observer) 代替。

```
export default Component({
  options: {
    pureDataPattern: /^_/
  },
  properties: {
    a: Boolean,
    _b: {
      type: Boolean,
      observer() {
        // 不要这样做！这个 observer 永远不会被触发
      }
    },
  }
})
```

从小程序基础库版本 [2.10.1](compatibility) 开始，也可以在组件的 json 文件中配置 `pureDataPattern`（这样就不需在 js 文件的 `options` 中配置）。此时，其值应当写成字符串形式：

```
{
  "pureDataPattern": "^_"
}
```

