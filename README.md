# PenpotWorkflow

面向 Godot 游戏项目的开源 UI 原型工作流仓库。

本仓库用于沉淀一套以 [Penpot](https://penpot.app/) 为核心的 UI 原型与设计交付流程，目标不是替代 Godot 内的实际界面实现，而是解决下面几类问题：

- 在正式进入 Godot 开发前，先用开源工具完成界面结构、布局和交互草图验证
- 为程序、美术、策划提供统一的页面规格、组件命名和资源交付约定
- 降低“设计稿在外部工具里、实现细节在 Godot 里”之间的沟通损耗
- 为类似 Sideline 这类游戏项目建立可复用的 UI 原型方法论

## 仓库目标

- 建立适合游戏项目的 Penpot 使用约定
- 明确 Penpot 到 Godot 的协作边界
- 输出可复用的文档模板、命名规范和交付流程
- 支持从线框图到高保真原型，再到 Godot 实装的分阶段推进

## 非目标

- 不把 Penpot 直接当作 Godot UI 运行时
- 不追求自动生成完整 Godot UI 代码
- 不把本仓库变成通用设计系统平台

## 建议目录

当前仓库已初始化以下基础结构：

```text
PenpotWorkflow/
├── README.md
├── LICENSE
├── .gitignore
└── docs/
    ├── Overview.md
    ├── RepositoryStructure.md
    └── Workflow.md
```

后续建议按需要扩展：

```text
PenpotWorkflow/
├── docs/                  # 流程、规范、设计说明
├── templates/             # 文档模板、Penpot 页面模板说明
├── references/            # 参考案例、调研资料
├── exports/               # 从 Penpot 导出的交付物（建议按版本管理策略决定是否入库）
└── samples/               # Godot 对接示例、命名样例、组件映射示例
```

## 适用场景

- Godot 2D / 3D 游戏项目的 UI 线框与高保真原型
- 独立游戏、小团队项目的协作规范建设
- 需要优先使用开源工具而非 Figma 的工作流
- 希望为程序实现阶段准备更清晰的 UI 规格说明

## 核心思路

### 1. Penpot 负责原型与规格

Penpot 适合承担这些职责：

- 页面结构梳理
- 组件拆分与复用
- 交互流程说明
- 状态稿管理
- 视觉规格与导出标注

### 2. Godot 负责最终实现

Godot 内的 `Control`、主题、动画、输入逻辑、分辨率适配仍应在项目代码中实现。Penpot 输出的是“设计意图”和“交付资产”，不是最终运行时界面。

### 3. 文档先于自动化

在缺少稳定自动导出链路之前，先把命名、交付、校对流程固定下来，比急于做脆弱的代码生成更有价值。

## 快速开始

1. 阅读 [docs/Overview.md](docs/Overview.md) 了解仓库定位
2. 阅读 [docs/Workflow.md](docs/Workflow.md) 建立 Penpot 到 Godot 的工作流
3. 阅读 [docs/RepositoryStructure.md](docs/RepositoryStructure.md) 按约定扩展目录与文件
4. 根据你的 Godot 项目实际需求，补充组件库、页面模板和导出规范

## 推荐使用方式

- 把本仓库作为“方法库”与“规范库”
- 在具体游戏项目中引用这里的流程和命名规范
- 当流程稳定后，再考虑增加脚本、模板或示例工程

## 后续可补充内容

- Penpot 页面模板
- 游戏 HUD / 弹窗 / 背包 / 商店等常见界面组件规范
- Godot `Control` 节点映射示例
- 资源导出脚本与命名校验脚本
- 设计评审清单与交付检查清单

## 许可

本仓库使用 MIT License，详见 [LICENSE](LICENSE)。
