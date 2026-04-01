# 仓库结构建议

## 设计原则

这个仓库应该优先保存“规范、模板、参考、样例”，而不是一上来塞进大量零散导出文件。目录如果从第一天就失控，后面很难变成可复用的方法库。

## 建议目录

```text
PenpotWorkflow/
├── README.md
├── LICENSE
├── .gitignore
├── docs/
│   ├── Overview.md
│   ├── Workflow.md
│   └── RepositoryStructure.md
├── templates/
│   ├── pages/
│   ├── components/
│   └── checklists/
├── references/
│   ├── games/
│   └── articles/
├── samples/
│   ├── naming/
│   └── godot-mapping/
└── exports/
    ├── release/
    └── review/
```

## 目录职责

### `docs/`

存放流程说明、命名规范、交付约定、设计评审清单等长期有效的文档。

### `templates/`

存放可复用的模板，例如：

- 页面命名模板
- 组件规格模板
- 交付检查清单
- 评审记录模板

### `references/`

存放参考案例、竞品截图整理、阅读笔记、链接归档等辅助材料。

### `samples/`

存放示例，而不是正式资产。建议包含：

- Penpot 页面命名示例
- 设计组件到 Godot 节点的映射示例
- 小规模 Godot UI 实现对照样例

### `exports/`

存放导出交付物。建议按用途拆分：

- `review/`：用于设计评审的临时导出
- `release/`：用于进入程序实现阶段的正式交付

如果导出文件量很大，可以只保留清单和版本记录，不直接提交二进制资源。

## 文件命名建议

### 文档文件

- 使用 PascalCase 或 kebab-case，保持全仓统一
- 示例：`Workflow.md`、`godot-mapping-example.md`

### 导出文件

建议包含三个维度：

- 页面或组件名
- 状态名
- 版本号或日期

示例：

- `inventory-panel-default-v001.png`
- `hud-combat-low-health-2026-04-01.png`

## 版本管理建议

- 规范文档必须入库
- 样例文件应尽量轻量
- 体积较大的导出资源要先明确是否真的需要纳入 Git
- 如果后期引入 Git LFS，应优先用于正式交付资源，而不是评审阶段临时文件
