# LYBTZYZS需求阶段架构守护Skill

## 📋 元数据

- **Skill名称**: lybtzyzs-requirements-arch-guard
- **版本**: v1.0
- **创建日期**: 2025-10-24
- **适用项目**: LYBTZYZS
- **触发场景**: 创建或更新需求文档时
- **优先级**: 🔴 强制执行（Mandatory）

## 🎯 核心目标

防止需求阶段遗漏架构约束，确保需求文档明确技术边界和架构限制，避免设计阶段的架构违规。

## ⚠️ 强制性文档阅读规则（⭐⭐⭐ 最高优先级）

**执行时机**：在生成或审查需求文档之前

### 规则1：拒绝未读文档的需求分析

**强制流程**：
1. **检测用户请求类型**：
   - 如用户要求"生成需求文档"、"写需求分析"、"创建Requirements文档"
   - **必须先拒绝**，提示："⚠️ 需求分析前必须先阅读文档体系，请确认是否已阅读相关文档？"

2. **强制文档阅读清单**：
   ```markdown
   📚 需求分析前必读文档（按优先级排序）：

   ### Level 0 - 导航中心（100%必读）
   - [ ] docs/index.md - 文档体系总入口，了解文档架构

   ### Level 1 - 核心规则（100%必读）
   - [ ] docs/business-rules.md - 14条核心业务规则（数据约束、业务流程、聚合根、计算规则、访问控制）
   - [ ] docs/architecture/{server|client|shared}/README.md - 对应层的架构指南

   ### Level 2 - 业务架构（根据功能必读）
   - [ ] docs/architecture/shared/clinical-workflow-entity-relationships.md - 看诊流程实体关系（医案/诊疗/处方）⭐⭐⭐
   - [ ] docs/explanation/medicalcase-consultation-prescription-enhancement-design.md - 三步工作流优化设计
   - [ ] docs/explanation/medicalcase-consultation-prescription-gap-analysis.md - 现有代码差距分析

   ### Level 3 - 模块文档（根据需求选择）
   - [ ] docs/reference/modules/{module-name}/README.md - 相关模块完整文档
   - [ ] docs/reference/api/{module-name}-api.md - 相关API文档
   - [ ] docs/reference/code-patterns-enhancement-summary.md - 代码模式参考

   ### Level 4 - 深度参考（可选）
   - [ ] docs/explanation/advanced-patterns.md - 7种设计模式实际应用
   - [ ] docs/explanation/api-design-best-practices.md - API设计最佳实践
   ```

3. **验证文档阅读**：
   - 使用Read工具实际读取核心文档（docs/index.md、business-rules.md、对应架构文档）
   - 生成文档要点摘要，包含：
     - 关键业务规则（如聚合根边界、数据约束）
     - 架构约束（如Write/Read Layer分离、技术黑名单）
     - 模块现状（如现有API、数据模型）
   - 向用户展示摘要，证明已理解
   - 用户确认后才继续需求分析

4. **生成需求文档**：
   - 基于阅读的文档体系，生成符合架构标准的需求文档
   - 需求文档必须包含"架构约束"章节
   - 需求文档必须引用相关架构文档和业务规则

### 规则2：需求文档必须包含架构约束章节

**强制内容**（使用本Skill的架构约束模板）：
- 核心架构原则（引用v2.0架构文档）
- 聚合根原则（Write/Read/Helper三层划分）
- 技术黑名单（Constitution约束）
- 设计阶段强制要求（架构合规性验证）
- 验收标准（架构层面）

### 违反处理：终止任务

**如果检测到以下违反行为，必须立即终止任务**：
- ❌ 未读取文档体系就生成需求文档
- ❌ 生成的需求文档未包含"架构约束"章节
- ❌ 需求文档未引用相关架构文档和业务规则
- ❌ 需求文档未要求设计阶段进行架构合规性验证

**终止提示**：
```
⚠️ 任务终止：违反需求分析强制性文档阅读规则

原因：[具体违反行为]
要求：必须先完成文档阅读流程
参考：.claude/skills/lybtzyzs-requirements-arch-guard/SKILL.md
参考：CLAUDE.md 第1.6节 - 强制性文档读取规则
```

---

## 🔍 背景与问题

### Epic #1589架构违规案例

**问题描述**：
- Epic #1589在需求文档中没有架构约束章节
- 导致设计阶段没有参考v2.0架构文档
- 最终产生9个架构违规，需要全面重构

**根本原因**：
- 需求文档缺少"架构约束"章节
- 没有引用相关架构文档
- 没有强制要求设计阶段进行架构合规性检查

**损失评估**：
- 开发时间：已实施的Phase 1功能需要重构（4-5小时）
- 设计返工：需求→设计→实施全部返工（15-21小时）
- 技术债务：引入9个架构违规，影响系统稳定性

## ✅ Skill功能

### 1. 需求文档架构约束检查

**检查项**：
- [ ] 是否有"架构约束"或"技术约束"章节
- [ ] 是否引用相关架构文档（如medicalcase-architecture-correction-plan-v2.md）
- [ ] 是否明确Write/Read Layer分离要求（如果涉及聚合根）
- [ ] 是否要求设计阶段必须进行架构合规性验证
- [ ] 是否列出技术黑名单（Constitution约束）

### 2. 架构文档关联性检查

**检查逻辑**：
1. 识别需求关键词（如"医案"、"诊疗"、"处方"）
2. 匹配对应的架构文档：
   - 医案/诊疗/处方 → `medicalcase-architecture-correction-plan-v2.md`
   - 患者管理 → `docs/architecture/server/患者模块.md`
   - 用户认证 → `docs/architecture/shared/authentication.md`
3. 检查需求文档是否引用对应架构文档

### 3. 强制性架构约束模板生成

**如果检测到缺失，自动生成模板**：

```markdown
## ⚠️ 架构约束（Architecture Constraints）

### 核心架构原则

**v2.0架构参考**：
- 📐 [MedicalCase聚合根架构v2.0](../architecture/shared/medicalcase-architecture-correction-plan-v2.md)

**聚合根原则**（如果涉及MedicalCase/Consultation/Prescription）：
- ✅ **Write Layer**：所有写操作必须通过MedicalCase聚合根
  - ✅ 允许：`POST /api/v1/medicalcases/{id}/complete-step1`
  - ❌ 禁止：`POST /api/v1/consultations/{id}/complete-step1`
- ✅ **Read Layer**：独立查询操作可以通过子实体Repository
  - ✅ 允许：`GET /api/v1/consultations/{id}`
- ✅ **Helper Layer**：工具函数不修改聚合根状态

**技术黑名单**（Constitution约束）：
- ❌ 禁止使用：Redis、CQRS、MediatR、Docker、GraphQL（见Constitution）
- ❌ 禁止使用：ServiceLocator模式
- ✅ 必须使用：构造函数依赖注入、async/await、中文注释

### 设计阶段强制要求

**架构合规性验证**（Mandatory）：
- [ ] 设计文档必须包含"架构合规性验证"章节
- [ ] 设计完成后必须运行`lybtzyzs-arch-compliance` Skill
- [ ] 所有API端点设计必须符合Write/Read/Helper三层划分
- [ ] 设计文档必须引用本需求文档的架构约束章节

### 验收标准（架构层面）

除业务功能验收外，还需满足：
- [ ] 架构合规性检查：0违规（运行lybtzyzs-arch-compliance）
- [ ] API端点路径符合v2.0架构规范
- [ ] Service层职责清晰（Write vs Read Layer）
- [ ] Repository使用符合聚合根原则
```

## 🔄 工作流程

### 触发时机

**场景1：创建新需求文档**
```
用户：创建需求文档 docs/requirements/xxx-requirements.md
  ↓
Skill：自动检测需求关键词
  ↓
Skill：生成"架构约束"章节模板
  ↓
Skill：推荐需要引用的架构文档
  ↓
输出：完整的需求文档（包含架构约束）
```

**场景2：用户主动请求检查**
```
用户："检查需求文档的架构约束"
  ↓
Skill：读取需求文档
  ↓
Skill：检查架构约束章节是否存在
  ↓
Skill：检查架构文档引用是否正确
  ↓
输出：架构约束检查报告
```

### 执行步骤

**Step 1：读取需求文档**
- 使用Read工具读取`docs/requirements/*.md`

**Step 2：关键词识别**
```python
keywords_mapping = {
    "医案|诊疗|处方": "medicalcase-architecture-correction-plan-v2.md",
    "患者|病人": "患者模块架构.md",
    "用户|认证|登录": "authentication.md"
}
```

**Step 3：架构约束检查**
- 检查是否有"架构约束"或"技术约束"章节
- 检查是否引用对应的架构文档
- 检查是否有"设计阶段强制要求"

**Step 4：生成检查报告**
```markdown
## 需求文档架构约束检查报告

### 文档信息
- 文档路径：docs/requirements/xxx-requirements.md
- 检查时间：2025-10-24 10:30:00
- 关键词匹配：医案、诊疗、处方 → MedicalCase聚合根

### 检查结果

#### ✅ 通过项
- [x] 包含"架构约束"章节
- [x] 引用v2.0架构文档
- [x] 明确Write/Read Layer要求

#### ❌ 缺失项
- [ ] 缺少"设计阶段强制要求"
- [ ] 未列出技术黑名单

### 建议改进
1. 添加"设计阶段强制要求"章节
2. 引用Constitution技术黑名单
3. 添加架构验收标准
```

**Step 5：自动生成缺失章节**
- 如果检测到缺失，询问用户是否自动添加
- 生成符合模板的架构约束章节

## 📊 输出格式

### 1. 检查报告（Markdown）

**通过示例**：
```markdown
✅ 需求文档架构约束检查通过

文档：docs/requirements/medicalcase-enhancement-requirements.md
检查项：5/5通过
关联架构：medicalcase-architecture-correction-plan-v2.md

所有架构约束检查项已满足，可以进入设计阶段。
```

**失败示例**：
```markdown
❌ 需求文档架构约束检查失败

文档：docs/requirements/xxx-requirements.md
检查项：2/5通过

缺失项：
1. ❌ 缺少"架构约束"章节
2. ❌ 未引用v2.0架构文档
3. ❌ 缺少设计阶段强制要求

建议：运行 /generate-arch-constraints 自动生成缺失章节
```

### 2. 架构约束章节（Markdown）

见上文"强制性架构约束模板生成"部分。

## 🛠️ MCP工具使用

### 核心工具链
1. **Read**：读取需求文档内容
2. **Grep**：搜索关键词和章节标题
3. **Edit**：插入架构约束章节
4. **mcp__serena__find_symbol**：查找相关架构文档
5. **mcp__memory**：读取项目架构知识库

### 工具协同流程
```
Read(需求文档) 
  → Grep(搜索"架构约束"章节)
  → Grep(搜索关键词"医案|诊疗|处方")
  → Read(相关架构文档)
  → Edit(插入架构约束章节)
  → 输出检查报告
```

## 🎯 成功案例

### 案例1：正确的需求文档

**文档**：`docs/requirements/medicalcase-enhancement-requirements-v2.md`（假设修复后）

**架构约束章节**：
```markdown
## ⚠️ 架构约束

### v2.0架构参考
- [MedicalCase聚合根架构v2.0](../architecture/shared/medicalcase-architecture-correction-plan-v2.md)

### 聚合根原则
- ✅ Write Layer：通过MedicalCase聚合根
- ✅ Read Layer：可独立查询
- ❌ 禁止：绕过聚合根的Write操作

### 设计阶段强制要求
- [ ] 设计文档必须引用本架构约束
- [ ] 设计完成后运行lybtzyzs-arch-compliance
- [ ] API设计符合Write/Read Layer分离
```

**Skill检查结果**：
```
✅ 架构约束检查通过
所有检查项满足要求，可以进入设计阶段。
```

### 案例2：Epic #1589（反面教材）

**原始需求文档问题**：
```markdown
## ❌ 原需求文档缺失

docs/requirements/medicalcase-consultation-prescription-enhancement-requirements.md

缺失项：
1. 没有"架构约束"章节
2. 没有引用v2.0架构文档
3. 没有Write/Read Layer要求
4. 没有设计阶段强制验证要求
```

**如果使用本Skill**：
```
❌ 需求文档架构约束检查失败

建议：自动生成架构约束章节
执行：/generate-arch-constraints
```

**生成的架构约束章节**：
```markdown
## ⚠️ 架构约束

### v2.0架构参考
- [MedicalCase聚合根架构v2.0](../architecture/shared/medicalcase-architecture-correction-plan-v2.md)

### 聚合根原则（关键）
- ✅ Write Layer：所有写操作通过MedicalCase聚合根
  - CompleteStep1 → POST /medicalcases/{id}/complete-step1
  - ResetSteps → PUT /medicalcases/{id}/reset-consultation-steps
- ❌ 禁止：POST /consultations/{id}/complete-step1（绕过聚合根）

### 设计阶段强制要求
- [ ] 设计文档必须运行lybtzyzs-arch-compliance检查
- [ ] API设计必须符合Write/Read Layer分离
- [ ] 所有Write端点通过MedicalCaseController
```

**避免的问题**：
- ✅ 设计阶段会参考架构约束，避免API设计违规
- ✅ 设计完成后会运行arch-compliance检查，提前发现违规
- ✅ 避免9个架构违规，节省15-21小时返工时间

## ⚠️ 限制条件

1. **仅检查需求文档**：不检查设计文档（由lybtzyzs-design-arch-validator负责）
2. **不执行代码检查**：仅检查文档是否包含架构约束声明
3. **依赖关键词匹配**：如果需求使用非标准术语，可能无法准确匹配架构文档
4. **需要人工确认**：生成的架构约束章节需要用户审查和调整

## 🔗 相关Skill

| Skill | 职责 | 触发阶段 |
|-------|------|---------|
| **lybtzyzs-requirements-arch-guard** | 需求文档架构约束检查 | 需求阶段 |
| **lybtzyzs-design-arch-validator** | 设计文档架构合规性验证 | 设计阶段 |
| **lybtzyzs-arch-compliance** | 代码架构合规性检查 | 实施阶段 |

## 📝 使用示例

### 示例1：创建新需求文档时

```
用户：我要创建一个新需求文档，增强医案列表的搜索功能

Skill：检测到关键词"医案"，自动生成架构约束章节

输出：
✅ 已生成需求文档模板
包含章节：
- 功能概述
- 功能需求
- ⚠️ 架构约束（已自动生成）
- 验收标准

架构约束内容：
- 引用：medicalcase-architecture-correction-plan-v2.md
- Write Layer：查询功能属于Read Layer，可独立实现
- 设计阶段要求：必须运行lybtzyzs-arch-compliance
```

### 示例2：检查现有需求文档

```
用户：检查docs/requirements/xxx-requirements.md的架构约束

Skill：读取文档，执行检查

输出：
❌ 架构约束检查失败

缺失项：
1. 缺少"架构约束"章节
2. 未引用v2.0架构文档

建议操作：
1. 添加架构约束章节（/add-arch-constraints）
2. 引用相关架构文档
3. 重新运行检查
```

## 🎯 预期效果

### 短期效果（1个月内）
- ✅ 100%的新需求文档包含架构约束章节
- ✅ 设计阶段违规率降低80%
- ✅ 避免类似Epic #1589的架构返工

### 长期效果（3个月后）
- ✅ 架构约束成为需求文档标准章节
- ✅ 团队成员自觉参考架构文档
- ✅ 架构违规率接近0
- ✅ 需求→设计→实施流程更顺畅

---

**维护者**：Claude Code
**最后更新**：2025-10-24
**反馈渠道**：GitHub Issues
