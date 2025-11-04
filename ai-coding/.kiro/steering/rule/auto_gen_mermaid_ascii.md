---
inclusion: always
---

# 设计文档图表生成强制规则

## 适用场景

当你执行以下任何操作时，本规则自动生效：
- 创建`.kiro/specs/*/design.md`文件
- 修改`.kiro/specs/*/design.md`文件
- 用户请求"更新design"、"生成design"、"创建design文档"

## 强制要求

### 要求1：design.md必须包含图表

你生成的design.md文件内部**必须**包含以下Mermaid图表：

1. **系统架构图** - 在"架构设计"章节使用```mermaid代码块
2. **业务流程图** - 在"业务流程"章节使用```mermaid代码块
3. **时序图** - 在"接口设计"或"交互流程"章节使用```mermaid代码块
4. **数据模型ERD图** - 在"数据模型"章节使用```mermaid代码块
5. **UI布局ASCII图** - 在"UI设计"章节使用```代码块

如果design.md中没有这些图表，你的工作是不完整的。

### 要求2：必须生成11个独立图表文件

在design.md所在的同一目录下，你**必须**创建以下11个文件：

1. `dd/design_diagram_usercase.md` - 用例图（Mermaid）
2. `dd/design_diagram_userstory.md` - 用户故事图（Mermaid）
3. `dd/design_diagram_flowchart.md` - 业务流程图（Mermaid）
4. `dd/design_diagram_sequence.md` - 时序图（Mermaid）
5. `dd/design_diagram_state.md` - 状态图（Mermaid）
6. `dd/design_diagram_component.md` - 组件交互图（Mermaid）
7. `dd/design_diagram_erd.md` - 实体关系图（Mermaid）
8. `dd/design_diagram_activity.md` - 活动图（Mermaid）
9. `dd/design_diagram_dfd.md` - 数据流图（Mermaid）
10. `dd/design_diagram_ipo.md` - IPO图（Mermaid）
11. `dd/design_diagram_ui.md` - UI布局图（ASCII）

每个文件必须包含：
- 文件标题
- 图表说明
- Mermaid或ASCII图表内容
- 如果design.md中没有相关内容，文件中写"无内容"

### 要求3：完成确认

生成完成后，你必须输出：

```
✓ design.md已包含5类图表
✓ 11个独立图表文件已生成
```

## 执行顺序

1. 先生成完整的design.md（包含内嵌图表）
2. 然后立即生成11个独立图表文件
3. 最后输出完成确认

## 重要提醒

这不是建议，是强制要求。每次处理design.md时都必须完成这些任务。
