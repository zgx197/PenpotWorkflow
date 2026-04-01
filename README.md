# Design to Facet

面向 Facet 与 Godot 的 AI 辅助 UI 设计工作流仓库。

本仓库用于沉淀一套从设计工具到 Facet，再到 Godot 的 UI 协作方式。它的目标不是替代 Godot 内的最终界面实现，而是帮助团队把设计意图、页面结构、组件约定和运行时承接协议连接起来，解决下面几类问题：

- 在正式进入 Godot 开发前，用设计工具和结构化约定验证页面布局、层级和交互草图
- 为程序、美术、策划和 AI 提供统一的页面命名、组件协议和交付边界
- 降低“设计稿在外部工具里、运行时结构在 Facet / Godot 里”之间的沟通损耗
- 为类似 Sideline 这类项目建立可复用的 UI 设计到运行时的工作流方法论

## 仓库目标

- 建立适合游戏项目的设计工具到 Facet 的协作约定
- 明确设计工具、Facet 和 Godot 之间的职责边界
- 输出可复用的文档模板、命名规范和交付流程
- 为后续的页面 schema、组件协议和 AI 辅助导入打基础

## 非目标

- 不把设计工具直接当作 Godot UI 运行时
- 不追求自动生成完整 Godot UI 逻辑
- 不让 AI 在无约束条件下直接替代页面工程实现
- 不把本仓库变成一个通用设计系统平台

## 建议目录

当前仓库已初始化以下基础结构：

```text
design-to-facet/
├── README.md
├── LICENSE
├── .gitignore
└── docs/
    ├── Overview.md
    ├── RepositoryStructure.md
    ├── Workflow.md
    ├── FacetDesign.md
    └── FacetAIDirection.md
```

后续建议按需要扩展：

```text
design-to-facet/
├── docs/                  # 流程、规范、设计说明
├── templates/             # 页面、组件、schema 与检查清单模板
├── references/            # 参考案例、调研资料、映射记录
├── samples/               # Facet 页面定义、组件协议、Godot 对接样例
└── exports/               # 评审预览、设计导出、交付清单
```

## 适用场景

- Godot 2D / 3D 游戏项目的 UI 线框、高保真和结构化交付
- 独立游戏、小团队项目的设计到运行时协作规范建设
- 需要同时支持 Penpot、Figma 或 AI 生成结构输入的工作流
- 希望为 Facet / Godot 实现阶段准备更清晰的页面结构与组件协议

## 核心思路

### 1. 设计工具负责表达意图与结构

Penpot、Figma 或其他设计工具适合承担这些职责：

- 页面结构梳理
- 组件拆分与复用
- 状态稿管理
- 交互流程说明
- 视觉规格与资源导出说明

### 2. Facet 负责承接页面协议

Facet 适合承担这些职责：

- 页面定义
- 节点 key 约定
- 动态布局 schema
- Binding 承接
- Lua 行为宿主
- 诊断与热重载协作

### 3. Godot 负责最终运行时实现

Godot 内的 `Control`、主题、动画、输入逻辑、分辨率适配和性能调优，仍应在项目代码中实现。Facet 负责把设计结构与运行时骨架接起来，而不是替代 Godot 的最终页面工程。

### 4. 文档与协议先于自动化

在缺少稳定导入链路之前，先把命名、交付、映射和校对流程固定下来，比急于做脆弱的自动生成更有价值。先统一协议，再逐步引入 AI 和导入器，风险更低。

## 快速开始

1. 阅读 [docs/Overview.md](docs/Overview.md) 了解仓库定位
2. 阅读 [docs/Workflow.md](docs/Workflow.md) 建立设计工具到 Facet / Godot 的工作流
3. 阅读 [docs/FacetDesign.md](docs/FacetDesign.md) 理解 Facet 的承接边界和运行时结构
4. 阅读 [docs/FacetAIDirection.md](docs/FacetAIDirection.md) 了解后续 AI 辅助导入方向
5. 根据你的项目实际需求，补充组件模板、页面 schema 和 Godot 对接样例

## 推荐使用方式

- 把本仓库作为“方法库”和“规范库”
- 在具体游戏项目中引用这里的流程、页面命名和组件约定
- 当 Facet 侧结构协议稳定后，再考虑增加导入脚本、校验器和示例工程

## 后续可补充内容

- 页面 schema 模板
- 组件协议模板
- Facet 节点类型映射表
- Godot `Control` 节点映射示例
- 设计导入校验脚本与命名检查脚本
- 设计评审清单与交付检查清单

## 许可

本仓库使用 MIT License，详见 [LICENSE](LICENSE)。
