---
title: "触发与监听事件"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/event-system.html"
source_route: "/miniprogram/dev/framework/component-framework/event-system.html"
raw_html: "raw-html/component-framework/event-system.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/event-system.html](https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/event-system.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/component-framework/event-system.html`

# 触发与监听事件

事件系统是组件间通信的主要方式之一。组件可以触发任意的事件，组件的使用者可以监听这些事件。

关于事件的基本概念和用法，参见 [事件](../view/wxml/event) 。

## 监听事件

监听自定义组件事件的方法与监听基础组件事件的方法完全一致，例如：

```
<component-tag-name bind:myevent="onMyEvent" />
```

```
export default Page({
  onMyEvent: function(e){
    e.detail // 自定义组件触发事件时提供的 detail 对象
  }
})
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/yb77dvmT81bj)

## 触发事件

自定义组件触发事件时，需要使用 `triggerEvent` 方法，指定事件名、detail 对象和事件选项，例如：

```
<button bind:tap="onTap"> 点击这个按钮将触发 myevent 事件 </button>
```

```
export default Component({
  properties: {},
  methods: {
    onTap: function(){
      const myEventDetail = {} // detail对象，提供给事件监听函数
      const myEventOption = {} // 触发事件的选项
      this.triggerEvent('myevent', myEventDetail, myEventOption)
    }
  }
})
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/3X7d9vmI81bd)

触发事件的选项包括：

| 选项名 | 类型 | 是否必填 | 默认值 | 描述 |
| --- | --- | --- | --- | --- |
| bubbles | Boolean | 否 | false | 事件是否冒泡 |
| composed | Boolean | 否 | false | 事件是否可以穿越组件边界，为 false 时，事件将只能在引用组件自身的 Shadow 树上触发，不进入其他任何组件内部 |
| capturePhase | Boolean | 否 | false | 事件是否拥有 [捕获阶段](event-binding#事件的捕获阶段) |

关于 `bubbles` 和 `composed`，可以参考下面这个例子。

```
<!-- 页面 page.wxml -->
<another-component bind:customevent="pageEventListener1">
  <my-component bind:customevent="pageEventListener2"></my-component>
</another-component>
```

```
<!-- 组件 another-component.wxml -->
<view bind:customevent="anotherEventListener">
  <slot />
</view>
```

```
<!-- 组件 my-component.wxml -->
<view bind:customevent="myEventListener">
  <slot />
</view>
```

```
// 组件 my-component.js
export default Component({
  methods: {
    onTap: function(){
      // 不冒泡
      // （只会触发 pageEventListener2）
      this.triggerEvent('customevent', {})

      // 在 Shadow 树上冒泡
      // （会依次触发 pageEventListener2、pageEventListener1）
      this.triggerEvent('customevent', {}, { bubbles: true })

      // 在 Composed 树上冒泡
      // （会依次触发 pageEventListener2、anotherEventListener、pageEventListener1）
      this.triggerEvent('customevent', {}, { bubbles: true, composed: true })
    }
  }
})
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/WwEiFAma8zbv)

## 在独立任务中触发事件

`triggerEvent` 会同步触发事件。

有时候这会导致一些问题，例如，在属性 `observer` 或数据监听器中调用，可能会导致 [递归更新问题](data-updates#递归更新问题) 。此时，应该在一个独立的任务里触发事件，例如：

```
export default Component({
  properties: {
    foo: String,
  },
  observers: {
    foo() {
      // 如果直接在数据监听器中触发事件，可能会导致递归更新问题
      // 此时，可使用 nextTick 来在独立任务中触发事件
      wx.nextTick(() => {
        this.triggerEvent('customevent', {})
      })
    },
  },
})
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/fD7nevmP81bJ)

