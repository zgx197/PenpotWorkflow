# Facet 整体设计

## 文档目的

本文档用于把 `Sideline` 项目中 `godot/scripts/facet` 这套框架的整体设计拆解清楚，并说明它为什么值得作为后续 UI 工具链与设计导入能力的承接基础。

这里讨论的不是某几个 Godot 控件脚本，而是一套已经具备运行时、绑定、布局、控制器、诊断和热重载能力的客户端表现层框架。

## Facet 的定位

Facet 不是：

- 业务逻辑框架
- ECS 框架
- 网络框架
- 单纯的 Godot UI 工具函数集合

Facet 是：

- 客户端页面运行时
- Projection 驱动的 UI 组织层
- 页面布局来源的统一承接层
- Lua 页面控制器宿主
- Binding、红点、热重载、诊断等共性能力的沉淀层

如果把客户端分层简化表达，可以写成：

```text
Domain / Lattice
    -> Application
    -> Projection
    -> Facet Runtime
        -> Layout
        -> Node Registry / Resolver
        -> Binding
        -> Lua Controller
        -> Extensions / Diagnostics
    -> Godot Scene / Node / Asset / Animation
```

这意味着 Facet 解决的核心问题不是“怎么画一个按钮”，而是“怎样让页面系统从数据、结构、行为到调试都具备统一协议”。

## 核心设计目标

结合现有代码和 README，可以把 Facet 的设计目标归纳为五条。

### 1. 页面层不直接依赖底层逻辑对象

页面不应该直接拿业务对象或引擎对象硬写 UI，而应该消费面向展示的 Projection 或已经整理过的状态数据。

这样做的价值在于：

- 降低页面对底层对象结构变化的敏感度
- 让 UI 更适合局部刷新和测试
- 让未来的 AI 生成和外部导入有稳定的数据消费层

### 2. 页面行为和页面结构分离

Facet 明确把页面拆成几部分：

- 页面定义
- 布局来源
- 节点注册和解析
- Binding
- 控制器行为

布局负责“页面长什么样”，控制器负责“页面做什么”，Binding 负责“数据和节点如何连接”。

这比把所有逻辑都塞进某个 Godot 场景脚本里更利于替换和自动化。

### 3. 多种布局来源进入统一运行时

Facet 不是只支持手工场景。

当前它已经有三类布局来源：

- ExistingNode / Scene 类布局
- Template 布局
- Generated 布局

无论布局来源是哪一种，最终都被收敛到统一的：

- `Control` 根节点
- `UINodeRegistry`
- `UINodeResolver`
- `UIPageRuntime`

这件事非常重要，因为它让“人工做出来的页面”和“结构化生成出来的页面”能够共用一套 runtime。

### 4. 页面行为允许逐步迁移到 Lua

Facet 并不是把 Lua 当作无边界脚本语言使用，而是通过受限桥接 API 让 Lua 承接页面行为层。

这样做的意义有三点：

- 页面交互逻辑可以脱离部分 C# 编译周期
- 页面层更适合热重载和快速试错
- 行为层边界更容易被工具和 AI 利用

### 5. 诊断和工具化不是附属品

Facet 没有把调试能力留到最后临时补，而是把运行时诊断、热重载测试、页面注册快照、红点和 Projection 观察都当成正式能力。

这说明它不是只追求“跑起来”，而是追求“能观察、能定位、能验证”。

## 运行时主链路

Facet 的实际运行主链路可以概括为：

```text
FacetHost
    -> RegisterCoreServices
    -> UIPageRegistry / UIPageLoader / UIManager
    -> Open(pageId)
    -> Load Layout
    -> Build Node Registry / Resolver
    -> Create UIPageRuntime
    -> Attach Binding Scope
    -> Create Lua Controller
    -> Show / Refresh / Hide / Dispose
```

它背后的设计含义如下。

### FacetHost 是宿主和装配根

`FacetHost` 负责：

- 组装服务
- 初始化运行时环境
- 注册页面定义和布局提供者
- 挂接 Lua、RedDot、Projection、Diagnostics 等能力
- 驱动运行时热重载轮询

它的职责不是写具体页面逻辑，而是把 Facet 变成一个完整可运行的宿主。

### UIManager 是页面调度中心

`UIManager` 负责：

- 根据 `pageId` 打开页面
- 管理当前页面
- 管理返回栈
- 按缓存策略决定复用还是销毁页面 runtime

这标志着页面切换已经不是“某个节点 visible true/false”的原始做法，而是正式页面系统。

### UIPageRuntime 是页面生命周期壳子

`UIPageRuntime` 是 Facet 的关键抽象之一。它持有：

- 页面定义
- 页面根节点
- 节点注册表和解析器
- `UIContext`
- Binding 作用域
- Lua 控制器实例

它统一处理的生命周期包括：

- `Create`
- `Initialize`
- `Show`
- `Refresh`
- `Hide`
- `Dispose`

统一壳子的价值在于，后续不管页面来自手工场景、模板布局还是自动布局，都能进入同一套生命周期协议。

## 模块拆解

### Application

这一层负责命令和查询边界，不让页面直接穿透到底层系统。它提供：

- `ICommandBus`
- `IQueryBus`
- `AppResult`
- `IAppService`
- `IGateway`

这使得页面行为层可以依赖应用协议，而不是依赖底层对象结构。

### Projection

Projection 层负责把“领域状态”整理成“页面可消费的数据”。

它的意义在于：

- 页面读 Projection，而不是读业务对象
- 刷新可以从 Projection 变化驱动
- 红点、列表、显隐态、可交互态等都能有统一数据源

如果后续要接 AI 或设计导入，这一层也会是稳定输出的重要依赖。

### Runtime

Runtime 层主要包含：

- `FacetHost`
- `UIManager`
- `UIRouteService`
- `UIPageRuntime`
- `UIContext`

这是页面系统的骨架层，负责运行、切换、缓存、生命周期和上下文注入。

### Layout

Layout 层的职责是把不同来源的布局统一成可消费结果。

当前设计已经明确支持：

- 手工场景
- 模板布局
- 自动生成布局

对后续 UI 工具链最有价值的，是自动生成布局这一支。它表明 Facet 已经具备“从结构化描述直接建 Godot Control 树”的能力。

### UI / Binding

Binding 层提供：

- 节点解析
- 文本绑定
- 显隐绑定
- 交互状态绑定
- 按钮命令绑定
- 简单列表绑定
- 复杂列表绑定
- 组件级 BindingScope

这里的关键不是“绑定了哪些控件”，而是 Facet 已经把 Binding 做成了 scope 化的正式协议。

也就是说，页面不是自己随手拿节点改值，而是在一套统一的绑定作用域里注册、刷新和释放绑定对象。

### Lua

Lua 在 Facet 中承担页面行为层职责，而不是底层系统控制职责。

它当前通过 `LuaApiBridge` 可以做的事包括：

- 读取页面参数
- 获取绑定桥和组件绑定桥
- 触发页面跳转
- 发命令和查查询
- 查询红点
- 维护页面级状态袋
- 执行绑定刷新

它明确不能直接触碰：

- 底层世界对象
- 未受约束的全局服务
- CLR 反射
- 任意 Godot 内部实现细节

这个边界是合理的，也是后续 AI 介入时非常重要的安全点。

### Extensions / Diagnostics

当前扩展和工具化能力已经包括：

- 红点系统
- Lua 热重载协调
- 运行时诊断快照
- 页面注册和活动 runtime 观察
- Projection / Lua / RedDot 摘要

这说明 Facet 已经从“概念框架”走到了“具备工程观察能力的运行时”。

## 节点系统设计

Facet 在节点系统上做了一个很关键的选择：稳定 key 优先，而不是 Godot 路径优先。

当前节点注册支持三类键：

- 节点名
- 相对路径键
- `facet_node_key` 元数据键

其中最重要的是稳定业务键，也就是 `facet_node_key`。

这个设计的好处是：

- 页面结构变化时，绑定不必完全跟着路径重写
- 生成布局也能给节点写入稳定标识
- 组件子树可以通过局部 resolver 暴露稳定局部键

这为“外部设计描述 -> Godot 节点树 -> 绑定协议”打下了很好的基础。

## 动态布局的真实价值

Facet 当前最值得重视的能力，不是 Lua，也不是红点，而是它已经有了受限的结构化布局描述。

当前动态布局 schema 的核心特点是：

- 用稳定 key 描述节点
- 节点类型是受控枚举，不是完全开放
- 样式参数是有限字段，不是完全自由的 CSS
- 最终由动态节点工厂构建成 Godot `Control` 树

这说明 Facet 已经有一种“中间表示”：

```text
Layout Definition
    -> Dynamic Node Factory
    -> Control Tree
    -> Node Registry / Resolver
    -> Binding / Runtime
```

这和网页思路最大的不同在于，Facet 的中间表示天然面向 Godot，而不是先转去浏览器语义再回译回来。

## 当前设计的优势

从工程角度看，Facet 当前最强的地方有这些。

### 1. 边界清晰

页面定义、布局、绑定、控制器、数据、诊断分层明确，没有全部堆在同一个 Godot 脚本里。

### 2. 运行时协议已经形成

页面切换、缓存、返回栈、生命周期、绑定释放、Lua 恢复都已经进入正式 runtime，不是临时约定。

### 3. 已经具备“可生成 UI”的基础

Facet 并不是纯手工场景系统，它已经能根据结构化描述生成布局，这为后续外部导入和 AI 辅助留下了空间。

### 4. 行为热更和结构承接分开了

布局和页面行为不是同一件事。布局可以来自场景或生成，行为可以来自 Lua 或 C# 生命周期，这种组合非常适合渐进演化。

### 5. 调试能力够工程化

没有诊断能力的 UI 框架很难持续演进。Facet 这方面做得比很多内部 UI 框架成熟。

## 当前设计的边界和不足

Facet 已经有很好的骨架，但如果要承接更强的设计导入能力，当前还有一些明显边界。

### 1. 动态布局 schema 还偏窄

当前支持的节点类型和样式字段较少，更适合验证链路和简单页面，还不足以表达复杂游戏 UI。

### 2. 页面定义和布局定义还偏内置

很多定义仍然通过内存注册和内置构造提供。对工具导入和 AI 生成来说，更理想的状态应该是外部文件化。

### 3. 组件层协议还不够突出

现有设计里已经有组件级 binding scope，但“组件定义”本身还没有被做成比页面更正式的资源层。

### 4. 视觉系统尚未形成完整 token 层

如果未来要让 AI 参与 UI 生成，仅靠节点结构不够，还需要更稳定的颜色、字体、间距、状态样式 token。

### 5. 设计工具导入链路还没有接上

Facet 已经有承接层，但设计工具导出、结构归一化、映射规则和校验器还没有形成闭环。

## 对当前仓库的意义

对 `PenpotWorkflow` 这个仓库来说，Facet 的存在说明一件很关键的事：

我们讨论的已经不是“Penpot 怎么交付一堆 PNG 给 Godot”，而是“怎样把设计意图变成一种能被 Facet 消费的结构化协议”。

这会直接影响后续仓库的方向。

如果没有 Facet，那么仓库重点会放在：

- 页面命名
- 导出素材
- 人工实现规范

但在已有 Facet 的情况下，仓库还应该开始关注：

- 页面 schema
- 组件 schema
- 设计节点到 Godot 节点的映射
- 节点 key 生成规则
- 可自动导入的约束集

## 当前结论

Facet 的整体设计是成立的，而且完成度已经明显超过“实验性质的 UI 框架”。

可以把它理解成：

- 一套正式页面 runtime
- 一套受控的布局承接协议
- 一套稳定的节点绑定系统
- 一套可热重载的页面行为宿主
- 一套具备诊断能力的客户端表现层基础设施

它最值得继续发展的地方，不是再堆更多页面代码，而是把“结构化 UI 描述”这一层正式做出来，并让设计工具和 AI 可以稳定地喂给它。
