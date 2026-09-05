---
title: "组件初始化策略"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/init-strategy.html"
source_route: "/miniprogram/dev/framework/component-framework/init-strategy.html"
raw_html: "raw-html/component-framework/init-strategy.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/init-strategy.html](https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/init-strategy.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/component-framework/init-strategy.html`

# 组件初始化策略

> 仅 glass-easel 组件框架支持。

组件实例初始化流程有两种。

第一种初始化流程是 **以组件初始值来初始化** ，具体流程是：

1. 以组件自己定义的数据应用到 WXML 模板上；
2. 触发 created 生命周期；
3. 应用从 WXML 传来的属性值（对于页面根组件，就是从页面的 query 传来的属性值）；
4. 如果改变的属性会触发的数据监听器，那就把触发对应的数据监听器；
5. 将改变后的属性值应用到 WXML 模板上。

第二种初始化流程是 **以组件属性值来初始化** ，具体流程是：

1. 应用从 WXML 传来的属性值（对于页面根组件，就是从页面的 query 传来的属性值）；
2. 如果改变的属性会触发的数据监听器，那就把触发对应的数据监听器；
3. 将组件自己定义的数据连同改变后的属性值一同应用到 WXML 模板上；
4. 触发 created 生命周期。

组件的后续逻辑（如触发 attached 生命周期等），在这两种初始化流程上没有区别。下表是对两种初始化流程的主要区别的对比。

| 比较项 | 以组件初始值来初始化 | 以组件属性值来初始化 |
| --- | --- | --- |
| created 生命周期触发时的属性值 | 组件自己定义的属性初始值 | 从 WXML（或页面 query）传来的属性值 |
| 数据监听器先于 created 触发？ | 不可能 | 有可能 |
| 性能 | 较差 | 较好 |

glass-easel 目前以前者为默认的初始化方式，这种初始化方式具有更单一的时序，但是性能会相对差一些（因为可能多一次 WXML 模板更新）。

如果想追求更好的性能，可以设置 `propertyEarlyInit` 选项来改为后者：

```
export default Component()
  .options({
    // 以组件属性值来初始化
    propertyEarlyInit: true,
  })
  .data(() => ({
    a: 1,
  }))
  .init(function ({ lifetime, observer }) {
    observer('a', () => {
      // 可能早于 created 触发
    })
    lifetime('created', () => {
      // 可能晚于 observer 触发
    })
  })
  .register()
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/Bs42HBm58Tbs)

