---
title: "组件生命周期"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/lifetimes.html"
source_route: "/miniprogram/dev/framework/component-framework/lifetimes.html"
raw_html: "raw-html/component-framework/lifetimes.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/lifetimes.html](https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/lifetimes.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/component-framework/lifetimes.html`

# 组件生命周期

## 普通生命周期

当组件发生一些关键状态变化的时候，如被添加到页面中、从页面中移除等，会触发对应的生命周期回调函数。

其中，最重要的生命周期是 `attached`。它表示组件实例被添加到页面内；对于页面根组件，它表示整个页面创建完成并即将被展示。

Definition 形式的生命周期回调函数：

```
export default Component({
  lifetimes: {
    // attached 生命周期回调函数
    attached() {
      // 添加到页面时触发
    },
  },

  // 传统写法（不推荐）
  attached() {
    // 添加到页面时触发
  },
})
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/xz8Tdvm48Ubj)

Chaining 形式的生命周期回调函数：

```
export default Component()
  .lifetimes('attached', function () {
    // ...
  })
  .register()
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/tT8kevmx8Ubc)

目前提供的生命周期回调函数如下表所示。

| 生命周期 | 触发时机 | 触发次数 | 注意事项 |
| --- | --- | --- | --- |
| `created` | 组件实例刚刚被创建完时触发 | 每个实例触发一次 | 组件还未添加到页面节点树中，不能通过组件节点向上查找父节点或其他兄弟节点。 |
| `attached` | 组件实例被添加到页面后触发 | 每个实例最多触发一次 |  |
| `moved` | 组件实例在节点树中位置被移动后触发 | 次数不定 | 只有 `wx:for` 内项目可能触发。 |
| `beforeDetach` | 组件实例将被从页面内移除前触发 | 每个实例最多触发一次 | 组件将被移除，不再位于节点树中，不应再操作节点或更新数据。 |
| `detached` | 组件实例被从页面内移除后触发 | 每个实例最多触发一次 | 组件已被移除，不再位于节点树中，不应再操作节点或更新数据。 |
| `ready` | 组件准备就绪时触发 | 每个实例最多触发一次 | 表示组件在渲染线程完全渲染完毕 |
| `error` | 组件内的生命周期回调或事件回调抛出异常时触发 | 次数不定 | 回调参数为 `(err: unknown)` ，可用于组件级别的错误捕获 |

注意，`ready` 生命周期不推荐使用。因为它的回调触发可能会非常晚，并可能在组件被移除后才回调。绝大多数情况下，应使用 `attached` 代替。使用 `ready` 生命周期时，需要处理以下可能的情况：

- `attached` 触发后，`ready` 被触发；
- `attached` 未触发，仅 `ready` 被触发；
- `attached` 和 `detached` 依次触发后，`ready` 被触发。

除此以外，还有一些传统的 [页面生命周期回调函数](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page#生命周期回调函数) ，可以在页面根组件中使用。 [页面路由](../app-service/route) 文档中详细说明了部分生命周期的触发时序。

## 组件所在页面的生命周期

页面生命周期是一类特殊的生命周期。它在某个页面上触发时，会自动广播给所有页面内的所有组件。

| 生命周期 | 参数 | 描述 | 最低版本 |
| --- | --- | --- | --- |
| show | 无 | 组件所在的页面被展示时执行 | [2.2.3](../compatibility) |
| hide | 无 | 组件所在的页面被隐藏时执行 | [2.2.3](../compatibility) |
| resize | `Object Size` | 组件所在的页面尺寸变化时执行 | [2.4.0](../compatibility) |
| routeDone | 无 | 组件所在页面路由动画完成时执行 | [2.31.2](../compatibility) |

注意：自定义 tabBar 的 pageLifetime 不会触发。

Definition 形式的组件所在页面生命周期回调函数：

```
Component({
  pageLifetimes: {
    show: function () {
      // 页面被展示时触发
    },
  },
})
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/Bi8pgvm78UbU)

Chaining 形式的组件所在页面生命周期回调函数：

```
Component()
  .pageLifetime('show', function () {
    // 页面被展示时触发
  })
  .register()
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/9M8Ehvm28Ubx)

