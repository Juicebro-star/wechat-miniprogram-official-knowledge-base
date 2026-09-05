---
title: "组件脚本"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/scripting.html"
source_route: "/miniprogram/dev/framework/component-framework/scripting.html"
raw_html: "raw-html/component-framework/scripting.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/scripting.html](https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/scripting.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/component-framework/scripting.html`

# 组件脚本

组件脚本是页面的逻辑核心。它可以通过改变页面数据来最终决定界面上展示的内容。

组件脚本可以用 JavaScript 编写。不过，对于复杂的组件，更推荐使用 TypeScript 编写：[api-typings](https://github.com/wechat-miniprogram/api-typings) 库中包含重要的类型定义，可以用于规范代码编写。

无论创建多少个实例，组件脚本文件本身只会被执行一次。

在组件脚本中，必须使用 `Page` 构造器或 `Component` 构造器来定义组件（而且只能定义一次）。通常有以下几种定义方式，可任选其一：

- `Page` 构造器适合用来构造非常简单的页面；
- `Component` 构造器的 Definition 形式适合用来构造简单的自定义组件（包括页面）；
- `Component` 构造器的 Chaining 形式适合用来构造逻辑复杂的自定义组件（包括页面）、适合使用 TypeScript 编写。

在使用构造器时，推荐将它 `export default` 出来（尽管这不是必需的）。

## `Page` 构造器

对于简单的页面，可以使用 `Page` 构造器进行构造。它的结构更简洁，但不能使用某些复杂特性。

```
export default Page({
  data: {
    foo: 'bar',
  },
  onLoad() {
    // 页面创建时执行
  },
  onButtonTap() {
    // 用户事件触发时执行
  },
})
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/Pr7XXvmD81b6)

`Page` 构造器只能使用数据、简单生命周期和事件响应函数。如果需要使用其他组件框架特性，就必须使用 `Component` 构造器。

## `Component` 构造器的 Definition 形式

`Component` 构造器有两种使用形式：Definition 形式和 Chaining 形式。它们是可以混用的，实现时可以根据习惯任选其一。

```
export default Component({
  data: {
    foo: 'bar',
  },
  lifetimes: {
    attached() {
      // 页面创建时执行
    },
  },
  methods: {
    onButtonTap() {
      // 用户事件触发时执行
    },
  },
})
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/tY7GVvmE81bS)

如果想要将一个已经用 `Page` 构造的页面改用 `Component` 构造，只需要将简单生命周期和事件响应函数移入 `methods` 中。

## `Component` 构造器的 Chaining 形式

> Chaining 形式仅 glass-easel 原生支持。对于 exparser，需要使用 [chaining-api-polyfill](https://github.com/wechat-miniprogram/chaining-api-polyfill) 。

对于新代码，更推荐使用 Chaining 形式编写。因为 Chaining 形式更适合组织复杂的组件逻辑，对 TypeScript 的支持也更好。

```
export default Component()
  .data(() => ({
    foo: 'bar',
  }))
  .init(function ({ data, setData, lifetime, method }) {
    // 组件实例初始化时执行一次

    // 上方定义的 `data` 可以直接作为局部变量使用（不需要写成 `this.data`）
    // `setData` 亦然（不需要写成 `this.setData`）

    lifetime('attached', () => {
      // 页面创建时执行
      data.foo === 'bar' // true
    })

    const buttonTap = method(() => {
      // 用户事件触发时执行
      setData({ foo: 'new bar' })
    })

    return {
      buttonTap,
    }
  })
  .register()
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/uA7OUvm981bm)

### Chaining 形式的实例局部变量

Chaining 形式的 `.init(...)` 是一个实例初始化函数，它对于每个实例执行一次。

在这个函数里，可以自由定义局部变量。这些变量在实例之间是相互独立的。这也就是 Chaining 形式最大的优势。

```
// 在 JS 文件中的全局量是所有实例共享的
const sharedVar = 'shared'

export default Component()
  .init(function () {
    // 在 init 中的变量是每个实例专属的
    const instanceVar = 'instance'
  })
  .register()
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/737aTvmL81bd)

### Chaining 形式的逻辑拆分与形式混用

Chaining 形式中，链式方法可以多次调用。可以借此将复杂的组件逻辑自然拆分为几个部分。

```
export default Component()
  // 组件逻辑单元 A
  .data(() => ({
    appleCount: 0,
    applePrice: 1.2,
  }))
  .init(function ({ data, setData, method }) {
    const buyApple = method(() => {
      setData({ appleCount: data.appleCount + 1 })
    })
    return { buyApple }
  })

  // 组件逻辑单元 B
  .data(() => ({
    bananaCount: 0,
    bananaPrice: 3.4,
  }))
  .init(function ({ data, setData, method }) {
    const buyBanana = method(() => {
      setData({ bananaCount: data.bananaCount + 1 })
    })
    return { buyBanana }
  })
  .register()
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/9g73Qvmf81bX)

也可以将 Definition 形式混入到 Chaining 形式中。

```
export default Component()
  // 组件逻辑单元 A（使用 Definition 形式）
  .definition({
    data: {
      appleCount: 0,
      applePrice: 1.2,
    },
    methods: {
      buyApple() {
        this.setData({
          data: this.data.appleCount + 1,
        })
      },
    },
  })

  // 组件逻辑单元 B
  .data(() => ({
    bananaCount: 0,
    bananaPrice: 3.4,
  }))
  .init(function ({ data, setData, method }) {
    const buyBanana = method(() => {
      setData({ bananaCount: data.bananaCount + 1 })
    })
    return { buyBanana }
  })
  .register()
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/qI7eRvmG81bo)

