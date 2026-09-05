# WeChat Mini Program Official Knowledge Base

一个面向 AI Coding、技术学习和小程序产品设计问答的微信小程序官方知识库。

这个项目把微信官方文档入口下的框架资料整理成可检索的 Markdown 页面、原始 HTML 快照和图片资产，让 AI 可以先理解官方平台规则，再结合具体应用仓库进行需求分析、方案设计、代码修改和验证。

> 本仓库是独立的资料归档与 AI Coding 适配项目，不是微信或腾讯官方仓库，也不代表官方立场。平台规则和当前版本应以官方页面为准。
<table>
  <tr>
    <td valign="top" width="68%">
      <strong>欢迎交流</strong><br />
      如果你在使用 Vibe Coding、AI Coding 或这个 Skill 的过程中有想法，欢迎联系果汁哥。<br /><br />
      <strong>微信号：</strong><code>guozhige2024</code><br />
      果汁哥有 AI 交流群，欢迎私信说明来意，申请加入。<br /><br />
      <a href="https://vibecoding.tan-xin.com/">访问 vibecoding base</a> ·
      <a href="https://github.com/Juicebro-star/vibecoding-base-skill">查看配套 Skill</a>
    </td>
    <td align="center" valign="top" width="32%">
      <a href="assets/juicebro-wechat-qr.png"><img src="assets/juicebro-wechat-qr.png" width="170" alt="果汁哥微信二维码" /></a><br />
      <sub>扫码添加微信 · 私信申请加入 AI 交流群</sub>
    </td>
  </tr>
</table>

## 现在能做什么

| 能力 | 当前状态 | 说明 |
| --- | --- | --- |
| 官方框架资料 | 已归档 | 305 个 `/miniprogram/dev/framework/` 页面 |
| Markdown 检索 | 已完成 | 按快速开始、逻辑层、视图层、运行时、能力、性能等目录组织 |
| 原始证据保留 | 已完成 | 305 个原始 HTML 页面保存在 `raw-html/` |
| 图片资产 | 已完成 | 105 个图片资产本地归档，其余保留官方 URL |
| AI 资料路由 | 已完成 | `AGENTS.md` 与 `AI_CONTEXT.md` 指导 AI 按问题检索 |
| AI Coding 配合 | 已完成 | 支持和具体小程序源码仓库一起使用 |
| 版本边界 | 已声明 | 这是 2026-09-05 快照，不是实时文档服务 |

当前版本更适合：

- 小程序框架知识问答；
- 页面、组件、生命周期、配置和运行时设计讨论；
- AI Coding 前的官方资料检索；
- 小程序学习、代码审查和问题定位；
- 为另一个具体应用仓库提供平台知识。

当前版本还不是：

- 一个可以直接运行的小程序模板；
- 完整的组件、API、服务端 API、云开发和开发者工具知识库；
- 微信官方的实时镜像或官方 SDK；
- 替代真实项目测试、真机验证和版本复核的自动编程系统。

## 使用主线

```text
一句自然语言需求
        ↓
AI_CONTEXT.md：判断问题属于哪个官方文档主题
        ↓
检索相关 Markdown 页面和官方链接
        ↓
区分文档事实、示例推断和待验证行为
        ↓
读取具体应用仓库的代码、配置和测试
        ↓
实现修改并运行项目自己的检查
        ↓
输出变更文件、官方依据、验证结果和未决风险
```

## 项目结构

```text
.
├── AGENTS.md                 AI Coding 行为约束
├── AI_CONTEXT.md             文档路由和项目协作规则
├── 00_目录.md                305 个官方页面的目录
├── quickstart/               快速开始
├── app-service/              逻辑层与页面服务
├── view/                     WXML、WXSS、事件与渲染
├── component-framework/      组件框架
├── runtime/                  运行时与 Skyline
├── ability/                  网络、存储、文件和设备能力
├── open-ability/             登录、分享、消息和开放能力
├── performance/              性能与诊断
├── raw-html/                 原始 HTML 快照
├── assets/                   图片资产
├── manifest.json             页面清单
└── asset-manifest.json       图片清单
```

## 本地使用

这是 Markdown 资料库，不需要安装 Node.js 或启动服务。可以直接用 Obsidian、VS Code、GitHub 网页或其他 Markdown 工具打开。

```bash
git clone <your-repository-url>
cd wechat-miniprogram-official-knowledge-base
```

先阅读：

```text
AGENTS.md
AI_CONTEXT.md
00_目录.md
```

## 和 AI Coding 一起使用

把本仓库和具体的小程序应用仓库同时提供给 AI Coding 工具。AI 应该：

1. 先阅读本仓库的 `AGENTS.md` 和 `AI_CONTEXT.md`；
2. 再阅读目标应用仓库的目录、配置、页面和测试；
3. 只检索与当前任务相关的官方页面；
4. 引用官方页面 URL，说明技术选择的依据；
5. 修改代码后运行目标应用仓库已有的检查和构建；
6. 明确哪些内容已经验证，哪些内容仍需开发者工具、模拟器或真机验证。

## 来源与更新

- 官方入口：<https://developers.weixin.qq.com/miniprogram/dev/framework/>
- 当前快照：2026-09-05
- 页面数量：305
- 原始 HTML：305 个
- 本地图片：105 个

微信开发文档会持续更新。重新使用时，应优先打开页面中的 `official_url`，核对当前版本、平台限制、隐私规则、审核规则和兼容性要求。

## 公开边界

本仓库只包含官方文档快照、官方公开链接、转换后的 Markdown、原始 HTML 和公开图片资产，不包含应用源码、凭证、AppID、私有接口、聊天记录、个人资料或其他项目上下文。

文档和相关素材仍受各自权利人适用的版权、商标、服务条款和再发布规则约束。本仓库不附带会误导使用者的宽松许可证；对外使用前请自行确认相应内容和素材的授权边界。

## 贡献

- 一个提交只解决一个主题；
- 不提交密钥、Cookie、AppID、私有接口或本机绝对路径；
- 更新页面时保留官方 URL 和抓取日期；
- 不把个人项目需求写入官方知识库；
- 发现官方文档变化时，说明变更页面和核对日期。
