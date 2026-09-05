---
title: "组件树访问"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/component-tree-visit.html"
source_route: "/miniprogram/dev/framework/component-framework/component-tree-visit.html"
raw_html: "raw-html/component-framework/component-tree-visit.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/component-tree-visit.html](https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/component-tree-visit.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/component-framework/component-tree-visit.html`

# 组件树访问

通过组件树访问接口，可以直接获得另一个自定义组件实例 `this`。这样就可以自由地操作多个组件。

需要注意的是，滥用这个接口可能导致组件间过度耦合。对于更好的组件间通信方式，请参考 [组件间交互概述](interaction) 。

想要正确理解组件树的概念，请仔细阅读 [组件框架模型](../custom-component/component-system) 。

## 获取组件实例

两个有引用与被引用关系的组件，通常可以称为父组件和子组件。自定义组件可以获得引用它的自定义组件（父组件）实例、所有它引用的自定义组件（子组件）实例。

### 获取父组件实例

通过 `this.selectOwnerComponent` 可以获得父组件实例 `this`。对于页面根组件，将返回 `null`。

```
export default Component({
  lifetimes: {
    attached() {
      const owner = this.selectOwnerComponent()
    },
  },
})
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/IY7Lxvm280bE)

如果使用 Chaining 形式，可以结合 [trait behaviors](trait-behaviors) 来进行逻辑解耦。

```
// common.ts

export interface IFooTrait {
  bar(): string
}

export const fooTrait = Behavior.trait<IFooTrait>()
```

```
// 父组件可以实现 fooTrait
export default Component()
  .init(function ({ implement }) {
    implement(fooTrait, {
      bar() {
        return 'bar'
      },
    })
  })
  .register()
```

```
// 子组件使用 trait behaviors 接口来访问父组件
export default Component()
  .init(function ({ lifetime }) {
    lifetime('attached', () => {
      const owner = this.selectOwnerComponent()!
      const fooImpl = owner.traitBehavior(fooTrait)!
      fooImpl.bar() // 'bar'
    })
  })
  .register()
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/XnEWHAm28ybX)

### 获取子组件实例

通过 `this.selectComponent` 可以获得自定义组件 Shadow 树中的任意自定义组件实例 `this`。

调用时需要传入一个选择器 `selector`，如：`this.selectComponent(".my-component")`。选择器详细语法可查看 [selector 语法参考文档](https://developers.weixin.qq.com/miniprogram/dev/api/wxml/SelectorQuery.select.html)。

另有一个类似的接口 `this.selectAllComponents`。他们的区别是，前者获取第一个匹配到选择器的节点，后者返回一个数组，包含所有匹配到的节点。

```
export default Component({
  lifetimes: {
    attached() {
      const comp = this.selectComponent(".my-component")
    },
  },
})
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/Re77Dvmj80bO)

在上例中，将会获取到 Shadow 树中 `class` 为 `my-component` 的子组件实例 `this`。

注意：默认情况下，小程序与插件之间、不同插件之间的组件将无法通过组件树访问接口得到组件实例（将返回 `null`）。如果想让一个组件在上述条件下依然能被 `selectComponent` 返回，可以自定义其返回结果（见下）。

### 自定义的组件实例获取结果

> 从基础库版本 [2.2.3](../compatibility) 开始提供支持。对于 exparser 组件框架，需要引入内置 behavior `wx://component-export` 这一功能才会生效。

`selectOwnerComponent` `selectComponent` `selectAllComponents` 返回的结果，可以通过 `export` 定义段来改变。

```
// 子组件 my-component 内部
Component({
  behaviors: ['wx://component-export'],
  export() {
    return { myField: 'myValue' }
  }
})
```

```
<!-- 使用自定义组件时 -->
<my-component id="the-id" />
```

```
// 父组件调用
const child = this.selectComponent('#the-id') // 等于 { myField: 'myValue' }
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/IW7aEvmL80bY)

在上例中，父组件获取 `id` 为 `the-id` 的子组件实例时，得到的是对象 `{ myField: 'myValue' }` 。

