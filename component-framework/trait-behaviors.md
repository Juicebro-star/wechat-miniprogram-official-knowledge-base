---
title: "trait behaviors"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/trait-behaviors.html"
source_route: "/miniprogram/dev/framework/component-framework/trait-behaviors.html"
raw_html: "raw-html/component-framework/trait-behaviors.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/trait-behaviors.html](https://developers.weixin.qq.com/miniprogram/dev/framework/component-framework/trait-behaviors.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/component-framework/trait-behaviors.html`

# trait behaviors

如果一个组件需要暴露一些 JS 接口给另一些组件使用，可以用 methods、export 等方式，但它们都有些这样那样的问题。

现在最推荐的方式是使用 trait behaviors。它可以有效解耦逻辑，并具有很好的 TypeScript 支持。

> 目前，trait behaviors 只有 glass-easel 组件框架原生支持。想在 exparser 组件框架上使用，可以借助 [chaining-api-polyfill](https://github.com/wechat-miniprogram/chaining-api-polyfill) 。但需要注意：chaining-api-polyfill 定义的 trait behaviors 只能在由它定义的组件中使用；反之，glass-easel 原生定义 trait behaviors 也只能在 glass-easel 原生定义的组件中使用。

## trait behaviors 基本用法

设计理念上，trait behaviors 主要是配合 TypeScript 类型使用的。因此，以下例子都用 TypeScript 编写。

### 定义 trait behaviors

首先，使用 trait behaviors 定义一组接口。这组接口可以有个 TypeScript 声明。

```
// 定义一组接口
export interface IFooTrait {
  bar(): string
}

// 将 TypeScript 接口定义包装成 trait behavior
export const fooTrait = Behavior.trait<IFooTrait>()
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/qq8KDvma8Tb9)

### 实现 trait behaviors

提供接口实现的组件，就需要实现 trait behaviors。实现时，需要使用 Chaining 形式。

```
export default Component()
  .init(function ({ implement }) {
    // implement 应在 init 函数返回前调用
    // 不应该写在生命周期函数、事件回调函数、其他方法内部
    implement(fooTrait, {
      bar() {
        return 'bar'
      },
    })
  })
  .register()
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/Bs8uGvmo8TbO)

### 调用 trait behaviors 实现

对于已经实现了 trait behaviors 的组件，通过这个组件的 `this` 可以访问到它的实现。

```
export default Component()
  .init(function ({ implement, lifetime }) {
    // 实现 trait behavior
    implement(fooTrait, {
      bar() {
        return 'bar'
      },
    })

    lifetime('attached', () => {
      // 获取组件实例 `this` 对 fooTrait 的实现（未实现的话，返回 undefined）
      const fooImpl = this.traitBehavior(fooTrait)!

      // 可以调用 trait behaviors 中的方法
      fooImpl.bar() // 'bar'
    })
  })
  .register()
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/E48JAvmx8TbI)

实践中，更常见的情况是 trait behaviors 配合 [组件间关系](../custom-component/relations) 或 [组件树访问](component-tree-visit) 来一同使用，请参考它们的文档。

## trait behaviors 的扩展方法

trait behaviors 本身可以包含一些扩展方法。这些扩展方法可以基于已有方法扩展出一些新的接口，有时可以用来简化代码。

具体来说，`Behavior.trait` 可以提供一个转换函数，例如：

```
// 实现者需要实现的方法
interface IPlusTrait {
  plus(a: number, b: number): number
}

// 调用者可以调用的方法
interface IPlusMinusTrait extends IPlusTrait {
  minus(a: number, b: number): number
}

// Behavior.trait 可以传递一个转换函数，将实现者实现的 impl 转换为调用者可以获取的对象
export const plusMinusTrait = Behavior.trait<IPlusTrait, IPlusMinusTrait>((impl) => ({
  // 添加一些调用者可以调用的方法
  minus(a: number, b: number) {
    return impl.plus(a, -b)
  },
  ...impl
}))
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/5vrW2wm88pby)

转换函数也可以裁剪掉一些不暴露给调用者的接口（不完全暴露 `...impl`）。

