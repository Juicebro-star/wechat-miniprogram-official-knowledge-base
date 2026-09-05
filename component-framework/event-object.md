---
title: "事件对象"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/event-object.html"
source_route: "/miniprogram/dev/framework/component-framework/event-object.html"
raw_html: "raw-html/component-framework/event-object.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/event-object.html](https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/event-object.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/component-framework/event-object.html`

# 事件对象

事件处理函数的第一个参数是一个事件对象。

所有事件对象都具有以下属性：

| 属性 | 类型 | 说明 | 基础库版本 |
| --- | --- | --- | --- |
| [type](#type) | String | 事件类型 |  |
| [timeStamp](#timeStamp) | Integer | 事件生成时的时间戳 |  |
| [target](#target) | Object | 触发事件的组件的一些属性值集合 |  |
| [currentTarget](#currenttarget) | Object | 当前组件的一些属性值集合 |  |
| [mark](#mark) | Object | 事件标记数据 | [2.7.1](../compatibility) |
| [detail](#detail) | Object | 额外的信息（在组件触发事件时提供） |  |

触摸和点击类事件额外具有以下属性：

| 属性 | 类型 | 说明 |
| --- | --- | --- |
| [touches](#touches) | Array | 触摸事件，当前停留在屏幕中的触摸点信息的数组 |
| [changedTouches](#changedTouches) | Array | 触摸事件，当前变化的触摸点信息的数组 |

## type

代表事件的类型。

## timeStamp

一个时间戳，表示页面打开到触发事件所经过的毫秒数。

## target

触发事件的源组件（即冒泡事件的起点）。以点击事件为例，它表示用户最终点击到的那个组件。

| 属性 | 类型 | 说明 |
| --- | --- | --- |
| id | String | 事件源组件的 id |
| [dataset](#dataset) | Object | 事件源组件的 dataset |

## currentTarget

事件绑定的当前组件。它指代 `bind` 或 `catch` 事件绑定所在的那个组件。

对于非冒泡事件，`currentTarget` 和 `target` 相同。

在实践中，大多数情况下使用 `currentTarget` 更符合预期。

| 属性 | 类型 | 说明 |
| --- | --- | --- |
| id | String | 当前组件的 id |
| [dataset](#dataset) | Object | 当前组件上的 dataset |

## 事件节点数据

在组件节点上可以附加一些自定义数据。这样，在事件中可以获取这些自定义的节点数据，用于事件的逻辑处理。

有两种方式为节点附件自定义数据：dataset 和 mark。现在更推荐使用 mark。

### dataset

对于 dataset，这些自定义数据以 `data-` 开头，多个单词由连字符 `-` 连接。这种写法中，连字符写法会转换成驼峰写法，而大写字符会自动转成小写字符。如：

- `data-element-type` ，最终会呈现为 `event.currentTarget.dataset.elementType` ；
- `data-elementType` ，最终会呈现为 `event.currentTarget.dataset.elementtype` 。

```
<view data-alpha-beta="1" data-alphaBeta="2" bind:tap="bindViewTap"> DataSet Test </view>
```

```
export default Page({
  bindViewTap:function(event){
    event.currentTarget.dataset.alphaBeta === 1 // 会转为驼峰写法
    event.currentTarget.dataset.alphabeta === 2 // 大写会转为小写
  }
})
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/R9exDBm180bc)

在 glass-easel 组件框架中，可以使用 `data:` 写法，这种写法不会进行任何转换，通常更容易理解。

```
<view data:alphaBeta="1" bind:tap="bindViewTap"> DataSet Test </view>
```

```
export default Page({
  bindViewTap:function(event){
    event.currentTarget.dataset.alphaBeta === 1
  }
})
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/4ze4wBm48QbY)

### mark

在基础库版本 [2.7.1](../compatibility) 以上，可以使用 `mark` 来识别具体触发事件的 target 节点。此外，`mark` 还可以用于承载一些自定义数据（类似于 `dataset`）。

当事件触发时，事件冒泡路径上所有的 `mark` 会被合并，并返回给事件回调函数（即使事件不是冒泡事件，也会有这样的 `mark` 合并）。这使得 `mark` 通常比 `dataset` 更实用。

```
<view mark:myMark="last" bind:tap="bindViewTap">
  <button mark:anotherMark="leaf" bind:tap="bindButtonTap">按钮</button>
</view>
```

在上述 WXML 中，如果按钮被点击，将触发 `bindViewTap` 和 `bindButtonTap` 两个事件，事件携带的 `event.mark` 将包含 `myMark` 和 `anotherMark` 两项。

```
export default Page({
  bindViewTap: function(e) {
    e.mark.myMark === "last" // true
    e.mark.anotherMark === "leaf" // true
  }
})
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/rze1zBmJ8Qbu)

`mark` 和 `dataset` 很相似，主要区别在于：`mark` 会包含从触发事件的节点到根节点上所有的 `mark:` 属性值；而 `dataset` 仅包含一个节点的 `data-` 属性值。

细节注意事项：

- 如果存在同名的 `mark` ，父节点的 `mark` 会被子节点覆盖。
- 在自定义组件中接收事件时， `mark` 不包含自定义组件外的节点的 `mark` 。
- 不同于 `dataset` ，节点的 `mark` 不会做连字符和大小写转换。

## touches

`touches` 是一个数组，每个元素为一个 `Touch` 对象，表示当前停留在屏幕上的触摸点。

`changedTouches` 与其类似，但是它包含所有变化的触摸点，包括从无变有（touchstart），位置变化（touchmove），从有变无（touchend、touchcancel）。

`Touch` 对象内部属性如下：

| 属性 | 类型 | 说明 |
| --- | --- | --- |
| identifier | Number | 触摸点的标识符 |
| pageX, pageY | Number | 距离文档左上角的距离，文档的左上角为原点 ，横向为X轴，纵向为Y轴 |
| clientX, clientY | Number | 距离页面可显示区域（屏幕除去导航条）左上角距离，横向为X轴，纵向为Y轴 |

对于 [canvas](https://developers.weixin.qq.com/miniprogram/dev/component/canvas.html) 组件生成的触摸类事件，Touch 对象还包含以下属性：

| 属性 | 类型 | 说明 |
| --- | --- | --- |
| x, y | Number | 距离 canvas 左上角的距离，横向为X轴，纵向为Y轴 |

## detail

表示事件的详细信息，它包含的字段根据事件的不同而不同。

自定义组件在 [触发事件](event-system#触发事件) 时可以指定这个字段的值。

点击类事件的 `detail` 带有的 x, y 同 pageX, pageY 代表距离文档左上角的距离。

