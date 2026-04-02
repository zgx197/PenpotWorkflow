# Stitch 能力模型与 design-to-facet 路线对照

## 文档目的

本文档用于对比 Google `Stitch` 的公开能力模型与本仓库 `design-to-facet` 的目标路线，明确两者的相同点、差异点、可借鉴部分，以及不应直接照搬的部分。

这里的重点不是评价谁更强，而是回答一个更实际的问题：

`Stitch` 这样的产品，对我们正在构建的设计到 Facet / Godot 工作流，到底意味着什么。

## 背景

根据 Google 官方公开信息：

- 2025-05-20，Google 在 I/O 开发者更新中介绍了 Stitch，强调它可以根据自然语言或图片提示生成高质量 UI 设计，并导出到 HTML / CSS 或 Figma
- 2026-03-18，Google 把 Stitch 往“AI-native software design canvas”推进，补充了设计代理、`DESIGN.md`、MCP server / SDK 和更强的设计协作能力
- 2026-03-27，Google 又提到基于 Gemini 3 的更新，提高了 UI 生成质量，并加入了 `Prototypes` 能力

这些信息说明，Stitch 的方向已经不只是“AI 画界面”，而是在往“可被 agent 和工具链消费的设计系统”演进。

## 一句话结论

可以把两者的关系概括为：

```text
Stitch 更像设计输入层和原型生成层
design-to-facet 更像 Facet / Godot 的结构承接层和运行时协作层
```

换句话说：

- Stitch 更偏设计语义、原型语义和前端语义
- design-to-facet 更偏页面协议、运行时结构和 Godot 语义

它们不是同一层，不构成直接替代关系。

## 对照总览

| 对比维度 | Stitch | design-to-facet |
| --- | --- | --- |
| 核心定位 | AI-native 设计与原型生成工具 | 设计到 Facet / Godot 的工作流与协议方法库 |
| 输入方式 | 文本提示、图片、设计规则、设计系统 | 设计工具结构、组件约定、页面 schema、AI 归一化输入 |
| 输出重点 | UI 设计稿、原型、前端代码、Figma、设计规则 | Facet 页面协议、节点 key 规则、组件约定、Godot 承接方式 |
| 中间语义 | 设计系统语义、前端语义、原型流语义 | 页面 schema、布局 schema、组件 schema、Facet 节点映射 |
| 目标运行时 | 浏览器、Figma、设计协作环境 | Facet runtime、Godot `Control`、Lua / Binding / Projection |
| AI 角色 | 直接生成设计和原型 | 在约束内把设计结构转换成 Facet 可消费协议 |
| 关注重点 | 设计速度、可视化探索、协作和 agent 设计能力 | 结构稳定性、运行时承接、可绑定、可调试、可落地 |
| 工具链位置 | 上游设计能力节点 | 中游协议和下游运行时桥梁 |

## 相同点

### 1. 都在做 AI 参与 UI 设计流程

两者都不再把 UI 理解成“先画图、再人工照着重写”，而是开始让 AI 直接参与结构整理、页面生成和工作流组织。

### 2. 都在强调结构化表达

Stitch 不是只给图片，它越来越强调：

- 设计规则
- 设计系统
- 可导出的设计描述
- agent 可消费的协作接口

`design-to-facet` 也在强调：

- 页面 schema
- 布局 schema
- 组件协议
- 节点 key
- 可被 Facet runtime 消费的结构化描述

### 3. 都在把设计从“静态稿”推进到“协议”

这是最值得注意的共性。

不管是 Stitch 的 `DESIGN.md`，还是我们未来的 `Facet UI Schema`，本质上都说明一件事：

仅靠静态图和口头约定，已经不足以支撑 AI 和工具链协作，必须有正式的中间表示。

## 关键差异

### 1. Stitch 偏设计与前端，design-to-facet 偏运行时承接

Stitch 公开强调的输出能力主要是：

- 高保真设计
- 原型流
- HTML / CSS
- Figma
- design rules

它并没有公开把重点放在：

- Godot 页面定义
- 节点 key
- Binding scope
- Lua 控制器
- Projection 驱动刷新

而这些恰恰是 `design-to-facet` 需要解决的核心问题。

### 2. Stitch 默认处理的是设计系统和交互原型，不是 Godot 语义

浏览器语义和 Godot 语义有重叠，但并不等价。

例如：

- CSS 布局不等于 Godot 容器布局
- HTML 结构不等于 Facet 的节点 key 结构
- Web 交互不等于游戏 UI 的输入与焦点系统
- 前端代码生成不等于 Godot 页面 runtime 接线

这意味着 Stitch 可以成为输入来源，但不能直接成为 Godot 运行时协议。

### 3. Stitch 的目标是设计效率，design-to-facet 的目标是运行时可承接

Stitch 首先关心的是：

- 设计探索快不快
- 原型生成快不快
- 设计系统能不能被复用
- agent 能不能参与协作

而 `design-to-facet` 首先关心的是：

- 页面结构是否稳定
- 节点 key 是否可绑定
- 布局是否能被 Facet runtime 承接
- Godot 实现阶段是否能减少重写

## Stitch 最值得借鉴的部分

### 1. `DESIGN.md` 思路

这可能是 Stitch 对我们最有价值的启发之一。

`DESIGN.md` 的意义不在于它是不是某个固定格式，而在于它代表一种思路：

- 设计规则应该是文本化、结构化、可被 agent 理解的
- 设计系统不应只存在于视觉稿里
- 设计约束应该能被导出、复用和协作

对 `design-to-facet` 来说，这个思路可以转成：

- 页面规则文档
- 组件协议文档
- 节点 key 规则文档
- token 规则文档
- Facet 节点映射规则文档

### 2. MCP / SDK 思路

Stitch 公开强调它可以通过 MCP server 和 SDK 接到别的工具链里，这说明它不想只做一个封闭产品，而是想成为 AI 工作流中的能力节点。

这对我们有两个启发：

- 后续的 `design-to-facet` 不应只停在文档层
- 页面 schema 和映射规则应该能被脚本、agent 和工具复用

换句话说，未来我们的路线也应考虑：

- 可被 AI 调用的页面 schema 生成器
- 可被工具调用的 Facet 校验器
- 可被编辑器消费的组件映射表

### 3. Prototype 流思路

Stitch 强调把静态界面串成原型流，这说明它在尝试把“单页设计”提升为“屏幕间交互关系”。

这点对我们也有借鉴意义，因为未来除了页面结构外，还应考虑：

- 页面跳转关系
- 弹窗唤起关系
- 页面状态切换
- 关键用户流程

这些内容在 Facet 侧可以沉淀成：

- 路由约定
- 页面入口参数约定
- 页面状态变体约定

### 4. 设计系统和 token 先行

Stitch 的很多能力实际上依赖设计系统和规则复用，而不是每个页面都从零开始生成。

这对我们意味着：

- 不应该让 AI 直接输出一堆散乱的页面结构
- 应该优先定义 token、组件、命名和结构约束

也就是说，AI 最该生成的是“约束内结果”，而不是“无约束创作”。

## 不该直接照搬的部分

### 1. 不要把 HTML / CSS 当成核心中间层

Stitch 很强的一部分能力是生成前端代码，但对我们来说，这条路只能作为参考，不适合作为主线。

原因很现实：

- Web 语义并不是 Godot 语义
- 前端结构转 Godot 会带来额外翻译层
- CSS 到 Facet / Godot 的映射难度高于设计结构到 Facet schema 的映射

所以我们不应该沿着：

```text
设计 -> Stitch -> HTML / CSS -> Godot
```

来定义主路线。

更适合我们的路线是：

```text
设计 -> 结构化设计数据 -> Facet UI Schema -> Facet Runtime -> Godot
```

### 2. 不要把设计代理直接等同于运行时代理

Stitch 里的设计 agent 更适合帮助整理页面结构、组件、样式和原型关系。

但 `design-to-facet` 还需要处理：

- 节点 key 稳定性
- Binding 作用域
- Projection 消费方式
- Lua 行为边界
- Godot 页面生命周期

这些已经不是纯设计代理可以直接覆盖的。

### 3. 不要把“生成效果好看”误当成“可落地”

Stitch 的可视化结果可能很强，但我们的目标不是只做一套“看起来像”的高保真草图，而是要做能被 Facet 和 Godot 稳定消费的结构。

对我们来说，生成质量的判断标准应该是：

- 结构是否清晰
- 命名是否稳定
- 节点是否可映射
- 页面是否可绑定
- 实现是否可演进

而不只是视觉是否足够惊艳。

## 对 design-to-facet 的实际启发

### 1. 我们应该明确自己的中间协议

Stitch 已经在做设计规则和设计系统层的结构化表达。

所以 `design-to-facet` 也不应只停在“流程说明”，而应逐步正式化这些内容：

- Page Schema
- Layout Schema
- Component Schema
- Token Schema
- Node Key Rules
- Facet Mapping Rules

### 2. 我们可以把 Stitch 视为潜在上游输入层

最合理的关系不是“拿 Stitch 替代 Facet”，而是：

```text
Stitch / Penpot / Figma
    -> 设计结构与设计规则
    -> 归一化
    -> Facet UI Schema
    -> Facet Runtime
    -> Godot
```

也就是说，Stitch 可以是设计输入层之一，但不应该成为 Godot 承接层本身。

### 3. 我们应优先做“受限转换”，而不是“自由生成”

从 Stitch 的演进也能看出，真正能落地的系统，最终都要回到规则、系统和协议，而不是只依赖一次性提示词生成。

这对我们意味着：

- AI 更适合做 schema 生成和修正
- AI 更适合做组件抽取建议
- AI 更适合做命名整理和映射补全
- AI 不适合在没有协议的前提下直接输出最终 Godot UI

### 4. 我们需要自己的“设计规则文档”

如果 Stitch 用 `DESIGN.md` 承载设计规则，那么 `design-to-facet` 也应该沉淀自己的等价物，例如：

- 页面命名规则
- 组件协议规则
- 节点 key 规则
- token 规则
- Facet 节点映射规则

这些内容不一定非要叫 `DESIGN.md`，但必须形成正式资产。

## 推荐的吸收方式

如果把 Stitch 视为行业参照系，而不是直接依赖对象，那么我们最适合吸收的东西有四类。

### 第一类：理念

- AI 不只是画图工具，而是设计协作工具
- 设计规则必须结构化
- 工作流应该可被 agent 和工具消费

### 第二类：协议意识

- 设计系统应可导出
- 设计规则应可复用
- 设计与实现之间应有正式中间层

### 第三类：工具链意识

- 上游设计工具应该能接入下游实现工具
- 中间表示应尽量稳定
- 校验器、导入器和预览器应成为正式能力

### 第四类：边界意识

- 设计生成不等于运行时实现
- 原型好看不等于工程可维护
- Web 语义不应直接替代 Godot 语义

## 最终判断

Stitch 和 `design-to-facet` 的关系，不应该理解为“Google 做了 Stitch，我们也应该做一个类似前端生成器”。

更准确的理解是：

- Stitch 证明了 AI-native UI 设计工作流是成立的
- Stitch 说明中间协议和设计规则文档非常重要
- Stitch 可以成为未来潜在的上游输入层之一
- `design-to-facet` 仍然需要坚持以 Facet / Godot 运行时承接为核心

如果把它们放到同一条链上，可以写成：

```text
Stitch 解决的是“设计如何更快地产生和组织”
design-to-facet 解决的是“这些设计结果如何被 Facet / Godot 稳定承接并落地”
```

## 参考信息

以下为撰写本文档时参考的 Google 官方公开信息：

- Google I/O 2025 developer updates，2025-05-20
- Introducing “vibe design” with Stitch，2026-03-18
- Stitch from Google Labs gets updates with Gemini 3，2026-03-27
- Google Labs: Stitch 产品页
