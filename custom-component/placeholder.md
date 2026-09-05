---
title: "占位组件"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/placeholder.html"
source_route: "/miniprogram/dev/framework/custom-component/placeholder.html"
raw_html: "raw-html/custom-component/placeholder.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/placeholder.html](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/placeholder.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/custom-component/placeholder.html`

# 占位组件

> 基础库 2.11.2 及以上版本支持，2.11.2 以下和未配置的效果相同

在使用如 [分包异步化](../subpackages/async) 或 [用时注入](../ability/lazyload#用时注入) 等特性时，自定义组件所引用的其他自定义组件，在刚开始进行渲染时可能处于未加载的状态。此时，为了使渲染过程不被阻塞，未加载的自定义组件需要一个 **占位组件**。基础库会用占位组件替代未加载的组件进行渲染，在该组件可用后再将占位组件替换回该组件。

目前，自定义组件未加载的情况包括：

- 使用分包异步化特性的情况下，引用了其他分包的组件，而对应分包还未下载；
- 使用用时注入特性的情况下，该组件还未注入。

## 配置

一个自定义组件的占位组件可以指定为另一个组件（自定义组件或内置组件都可以）。组件 JSON 配置中的 `componentPlaceholder` 字段用于指定占位组件，如：

```
{
  "usingComponents": {
    "comp-a": "../comp/compA",
    "comp-b": "../comp/compB",
    "comp-c": "../comp/compC"
  },
  "componentPlaceholder": {
    "comp-a": "view",
    "comp-b": "comp-c"
  }
}
```

该配置表示：

- 组件 `comp-a` 的占位组件为内置组件 `view`；
- 组件 `comp-b` 的占位组件为自定义组件 `comp-c`（对应路径在 `usingComponents` 中配置）。

假设该配置对应的模板如下：

```
<button ontap="onTap">显示组件</button>
<comp-a wx-if="{{ visible }}">
  <comp-b prop="{{ p }}">text in slot</comp-b>
</comp-a>
```

小程序启动时 `visible` 为 `false`，那么只有 `button` 会被渲染；点击按钮后，`this.setData({ visible: true })` 被执行，此时如果 `comp-a`, `comp-b` 都未加载，则页面（Shadow 树）将被渲染为：

```
<button>显示组件</button>
<view>
  <comp-c prop="{{ p }}">text in slot</comp-c>
</view>
```

`comp-a` 与 `comp-b` 准备完成后，页面（Shadow 树）被替换为：

```
<button>显示组件</button>
<comp-a>
  <comp-b prop="{{ p }}">text in slot</comp-b>
</comp-a>
```

## 注意事项

- 当一个组件被指定为占位组件时（如上例中的 `comp-c`），为其指定占位组件是无效的，换句话说，如果一个组件需要作为其他组件的占位组件，则它必须在一开始就是可用的；
- 如果一个组件未加载，且其占位组件不存在，则渲染时会报错并抛出；
- 如果一个组件不存在，但为其指定了可用的占位组件，则占位组件可以被正常渲染，但后续尝试准备替换时会报错并抛出。

### 附：有占位组件参与的渲染流程

基础库尝试渲染一个组件时，会首先递归检查 `usingComponents`，收集其将使用到的所有组件的信息；在这个过程中，如果某个被使用到的组件未加载，基础库会先检查其是否有对应的占位组件。如果没有，基础库会中断渲染并抛出错误；如果有，则会标记并在后续渲染流程中使用占位组件替换该未加载的组件进行渲染。未加载的组件会在当前渲染流程结束后尝试加载（下载分包或注入代码等）；等到准备过程完成后，再尝试渲染该组件（实际上也是在执行这个流程），并替换掉之前渲染的占位组件。

