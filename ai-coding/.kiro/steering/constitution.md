# 项目宪法 (Project Constitution)

**版本**: v1.0
**创建时间**: 2025-10-16
**适用范围**: 所有新功能开发、重构、优化
**强制级别**: MUST（违反将导致PR被拒绝）

---

## 📜 宪法目的

本文档定义LYBTZYZS项目的不可妥协原则，确保所有开发活动符合项目愿景、技术标准和质量要求。所有Spec必须经过Constitution合规性检查才能进入实施阶段。

---

## 1. 架构原则 (Architecture Principles)

### 1.1 三层对齐架构 (MUST)

**原则**：项目采用Server/Client/Shared三层架构，必须保持严格对齐。

**强制要求**：
- ✅ **Server端** - 三层架构（Controller → Service → Repository）
- ✅ **Client端** - MVVM五层架构（View → ViewModel → Service → ApiClient → Model）
- ✅ **Shared层** - 跨端共享组件（DTOs、Enums、Contracts）
- ❌ **禁止跨层直接调用** - 必须遵循依赖方向（UI → Service → Repository → DB）

**文档对齐**：
- 代码变更必须同步更新对应架构文档
- Server: `docs/architecture/server/README.md`
- Client: `docs/architecture/client/README.md`
- Shared: `docs/architecture/shared/README.md`

### 1.2 依赖注入规范 (MUST)

**原则**：仅使用构造函数依赖注入，禁止服务定位器模式。

**强制要求**：
- ✅ **构造函数注入** - 所有依赖通过构造函数注入
- ❌ **禁止ServiceLocator** - 禁止使用`Container.Resolve`、`ServiceProvider.GetService`
- ❌ **禁止属性注入** - 禁止使用`[Inject]`属性注入
- ✅ **接口驱动** - 依赖抽象接口而非具体实现

**示例**：
```csharp
// ✅ 正确
public class PatientService : IPatientService
{
    private readonly IPatientRepository _repository;
    private readonly ILogger<PatientService> _logger;

    public PatientService(
        IPatientRepository repository,
        ILogger<PatientService> logger)
    {
        _repository = repository;
        _logger = logger;
    }
}

// ❌ 错误
public class PatientService
{
    private IPatientRepository _repository = ServiceLocator.Resolve<IPatientRepository>();
}
```

### 1.3 技术黑名单 (MUST NOT)

**原则**：避免过度设计，禁止引入以下技术栈（除非MVP验证后有明确需求）。

**禁用技术**：
- ❌ **Redis** - 暂无分布式缓存需求
- ❌ **CQRS** - MVP阶段过度设计
- ❌ **MediatR** - 增加复杂度，无实际收益
- ❌ **Docker** - 部署暂无容器化需求
- ❌ **GraphQL** - RESTful API已足够
- ❌ **消息队列** (RabbitMQ/Kafka) - 无异步任务需求
- ❌ **微服务架构** - 单体架构更适合当前规模

**允许技术**：
- ✅ **.NET 8** - 主框架
- ✅ **EF Core** - ORM
- ✅ **SQL Server/SQLite** - 数据库
- ✅ **WPF** - 桌面客户端
- ✅ **JWT** - 认证机制

---

## 2. 代码质量标准 (Code Quality Standards)

### 2.1 测试覆盖率 (MUST)

**原则**：新增代码必须达到测试覆盖率要求。

**强制要求**：
- ✅ **核心业务逻辑** - 覆盖率 ≥ 80%
- ✅ **Service层** - 覆盖率 ≥ 75%
- ✅ **Repository层** - 覆盖率 ≥ 70%
- ✅ **ViewModel层** - 覆盖率 ≥ 60%（UI逻辑复杂度较低）

**测试优先级**：
1. 单元测试（Unit Tests） - 所有Service/Repository方法
2. 集成测试（Integration Tests） - API端点、数据库操作
3. E2E测试（可选） - 关键业务流程

### 2.2 文件大小限制 (SHOULD)

**原则**：保持代码文件精简，避免上帝类。

**推荐标准**：
- ⚠️ **单文件** - 建议 ≤ 500行（超过需要说明理由）
- ⚠️ **单函数** - 建议 ≤ 50行（超过需要拆分）
- ⚠️ **单类** - 建议 ≤ 300行（超过考虑拆分职责）

**例外情况**：
- ViewModel包含大量UI绑定属性
- DTO包含大量数据字段
- 代码生成文件（*.g.cs）

### 2.3 命名规范 (MUST)

**原则**：统一命名风格，提升代码可读性。

**强制要求**：
- ✅ **类/类型** - `PascalCase` (PatientService, ConsultationDto)
- ✅ **公开成员** - `PascalCase` (GetByIdAsync, PatientName)
- ✅ **私有字段** - `_camelCase` (_repository, _logger)
- ✅ **常量** - `UPPER_SNAKE_CASE` (MAX_RETRY_COUNT)
- ✅ **异步方法** - `Async`后缀 (CreateAsync, GetPagedAsync)
- ✅ **中文注释** - 所有代码注释使用中文
- ✅ **中文提交** - Git提交信息使用中文

### 2.4 文件编码 (MUST)

**原则**：统一文件编码，避免乱码问题。

**强制要求**：
- ✅ **UTF-8 with BOM** - 所有.cs、.xaml、.md文件
- ✅ **CRLF换行符** - Windows平台标准
- ✅ **无BOM** - .json、.yml配置文件

---

## 3. 开发流程规范 (Development Workflow)

### 3.1 Issue驱动开发 (MUST)

**原则**：所有代码变更必须先创建GitHub Issue，无Issue禁止任何改动。

**强制要求**：
- ✅ **所有功能开发** - 必须先创建Issue
- ✅ **所有Bug修复** - 必须先创建Issue
- ✅ **所有重构优化** - 必须先创建Issue
- ✅ **所有文档修正** - 必须先创建Issue
- ❌ **禁止无Issue工作** - 任何"顺手修改"都必须先创建Issue

**Issue模板**：
```markdown
## 📝 任务描述
[清晰描述要做什么]

## 🎯 目标
[要达成什么目标]

## ✅ 验收标准
- [ ] 标准1
- [ ] 标准2

## 📚 参考资料
[相关文档、设计方案]
```

### 3.2 Spec-Driven开发 (MUST for 新功能/重构)

**原则**：新功能和重大重构必须先完成Spec文档，获得审批后才能开发。

**工作流**：
```
Constitution检查 → Requirements → Design → Tasks → Checklist → Implementation
```

**必需文档**：
1. **requirements.md** - 需求定义（WHAT和WHY）
2. **design.md** - 设计方案（HOW）
3. **tasks.md** - 任务分解（实施步骤）
4. **checklists/** - 质量检查清单（验收标准）

**可选文档**：
- **research.md** - 技术调研（选型、对比）
- **data-model.md** - 数据模型（独立出来）
- **api-contracts.md** - API契约（OpenAPI格式）
- **quickstart.md** - 快速开始（测试指南）

**审批流程**：
- Dashboard审批 → requirements.md
- Dashboard审批 → design.md
- Dashboard审批 → tasks.md
- 所有checklist完成 → 允许PR合并

### 3.3 文档同步 (MUST)

**原则**：代码与文档必须同步更新，禁止滞后。

**强制要求**：
- ✅ **代码变更后立即更新文档** - 不允许延迟
- ✅ **架构变更必须更新架构文档**
- ✅ **API变更必须更新API文档**
- ✅ **配置变更必须更新配置文档**

**文档更新清单**：
- [ ] `docs/architecture/` - 架构设计变更
- [ ] `docs/development/` - 开发指南变更
- [ ] `docs/api/` - API接口变更
- [ ] `docs/quick-reference/` - 快速参考变更
- [ ] `README.md` - 项目概览变更

### 3.4 分支管理 (MUST)

**原则**：采用语义化分支命名，便于追踪和管理。

**分支命名规范**：
```
feature/{issue-id}-{description}  # 新功能
bugfix/{issue-id}-{description}   # Bug修复
refactor/{issue-id}-{description} # 重构
docs/{issue-id}-{description}     # 文档更新

示例:
feature/1344-formula-delayed-binding
bugfix/1401-prescription-price-calculation
refactor/1402-service-layer-optimization
```

**分支生命周期**：
1. 从master创建功能分支
2. 本地开发并提交
3. 创建PR关联Issue
4. 代码审查通过
5. 合并到master
6. 删除功能分支

---

## 4. 安全与合规 (Security & Compliance)

### 4.1 双轨认证系统 (MUST)

**原则**：普通用户与超级管理员物理隔离，避免权限混淆。

**强制要求**：
- ✅ **Users表** - 普通医生用户，无超级管理员角色
- ✅ **AdminSecrets表** - 超级管理员独立存储，物理隔离
- ❌ **禁止角色混用** - 超级管理员不存在于Users表
- ✅ **JWT双Token** - 普通用户Token与管理员Token分离

**实施细节**：
```csharp
// ✅ 正确：双轨认证
if (IsAdminRequest(request))
{
    await _adminAuthService.ValidateAdminTokenAsync(token);
}
else
{
    await _userAuthService.ValidateUserTokenAsync(token);
}

// ❌ 错误：统一认证
await _authService.ValidateTokenAsync(token); // 无法区分管理员
```

### 4.2 敏感数据保护 (MUST)

**原则**：敏感数据必须加密存储和传输。

**强制要求**：
- ✅ **密码加密** - BCrypt/PBKDF2哈希
- ✅ **超级管理员密钥** - AES256加密存储
- ✅ **患者隐私数据** - 符合医疗数据保护规范
- ✅ **HTTPS传输** - 生产环境强制HTTPS

### 4.3 审计日志 (SHOULD)

**原则**：关键操作必须记录审计日志。

**推荐记录**：
- ⚠️ **认证操作** - 登录、登出、Token刷新
- ⚠️ **数据变更** - 创建、更新、删除关键实体
- ⚠️ **权限操作** - 角色变更、权限授予
- ⚠️ **异常事件** - 登录失败、权限拒绝

---

## 5. MVP优先原则 (MVP-First Principle)

### 5.1 够用即好 (MUST)

**原则**：避免超前设计，专注MVP核心功能。

**强制要求**：
- ✅ **优先实现MVP定义的核心功能**
- ❌ **禁止私自扩展功能** - 需先更新MVP文档/Issue
- ❌ **禁止过度抽象** - 单一使用场景不需要抽象
- ❌ **禁止投机性优化** - 无性能瓶颈不需要优化

**决策标准**：
```
问：这个功能MVP需要吗？
  是 → 实现
  否 → 记录到待办清单，MVP后评估

问：这个设计模式必需吗？
  是 → 使用
  否 → 用最简单的方式实现
```

### 5.2 增量优化 (MUST)

**原则**：禁止推倒重写，采用增量优化策略。

**强制要求**：
- ✅ **小步快跑** - 每次改动控制在合理范围
- ✅ **保持向后兼容** - 避免破坏性变更
- ❌ **禁止大规模重构** - 需先创建Spec并获得批准
- ✅ **渐进式改进** - 逐步优化而非一次性重写

---

## 6. 文件组织规范 (File Organization)

### 6.1 根目录整洁 (MUST)

**原则**：禁止在根目录创建临时文件。

**强制要求**：
- ❌ **禁止根目录临时文件** - 文档/脚本/输出/截图
- ✅ **文档归档** - `docs/` 对应分类目录
- ✅ **脚本归档** - `scripts/` 功能目录
- ✅ **输出归档** - `docs/reports/` 或 `scripts/analysis/outputs/`
- ✅ **Pre-commit检查** - 自动检查根目录规范

### 6.2 文档分层架构 (MUST)

**原则**：遵循v5.0三层对齐文档架构。

**文档层次**：
- **Level 1** - 快速参考 `docs/quick-reference/` (80%日常需求)
- **Level 2** - 架构指南 `docs/architecture/` + `docs/development/` (15%学习需求)
- **Level 3** - 深度参考 `docs/deep/` + `docs/api/` + `docs/modules/` (5%深度需求)

**路径标准化**：
```
✅ docs/architecture/server/README.md
✅ docs/development/client/README.md
✅ docs/quick-reference/api-reference.md

❌ docs/server-architecture.md  # 旧路径，已废弃
❌ docs/client-dev-guide.md     # 旧路径，已废弃
```

---

## 7. 性能与资源 (Performance & Resources)

### 7.1 性能基准 (SHOULD)

**原则**：关键操作必须满足性能要求。

**推荐标准**：
- ⚠️ **API响应时间** - P95 < 200ms
- ⚠️ **UI操作响应** - < 100ms
- ⚠️ **数据库查询** - 避免N+1查询
- ⚠️ **内存使用** - 客户端 < 500MB

### 7.2 资源限制 (SHOULD)

**原则**：合理控制资源使用。

**推荐标准**：
- ⚠️ **数据库连接池** - 20-50连接
- ⚠️ **HTTP超时** - 30秒
- ⚠️ **文件上传** - 单文件 < 10MB
- ⚠️ **分页大小** - 默认20条，最大100条

---

## 8. 异常处理 (Exception Handling)

### 8.1 异常分层 (MUST)

**原则**：不同层次采用不同异常处理策略。

**强制要求**：
- ✅ **Controller层** - 捕获所有异常，返回标准错误响应
- ✅ **Service层** - 抛出业务异常（BusinessException）
- ✅ **Repository层** - 抛出数据异常（DataException）
- ✅ **全局异常处理** - 统一错误日志和响应格式

**示例**：
```csharp
// Service层
public async Task<PatientDto> GetByIdAsync(int id)
{
    var patient = await _repository.GetByIdAsync(id);
    if (patient == null)
    {
        throw new BusinessException($"患者不存在: {id}");
    }
    return _mapper.Map<PatientDto>(patient);
}

// Controller层
[HttpGet("{id}")]
public async Task<IActionResult> GetById(int id)
{
    try
    {
        var patient = await _patientService.GetByIdAsync(id);
        return Ok(patient);
    }
    catch (BusinessException ex)
    {
        return NotFound(new { message = ex.Message });
    }
    catch (Exception ex)
    {
        _logger.LogError(ex, "获取患者失败");
        return StatusCode(500, new { message = "服务器内部错误" });
    }
}
```

---

## 9. 违规处理 (Violation Handling)

### 9.1 强制级别定义

- **MUST** - 必须遵守，违反将导致PR被拒绝
- **SHOULD** - 强烈推荐，违反需要说明理由
- **MAY** - 可选，根据具体情况决定

### 9.2 审查流程

1. **Constitution检查** - 创建Spec时检查合规性
2. **代码审查** - PR审查时检查代码规范
3. **自动化检查** - Pre-commit hook检查部分规则
4. **人工审查** - 复杂规则需要人工判断

### 9.3 豁免机制

**特殊情况**可申请豁免：
1. 在PR中说明豁免理由
2. 提供替代方案
3. 记录技术债务
4. 计划后续优化

---

## 10. Constitution演进 (Evolution)

### 10.1 更新流程

1. 在GitHub Issue中提出修订建议
2. 团队讨论并达成共识
3. 更新Constitution文档
4. 通知所有开发人员
5. 更新相关工具和检查脚本

### 10.2 版本历史

| 版本 | 日期 | 变更内容 |
|------|------|----------|
| v1.0 | 2025-10-16 | 初始版本，基于spec-kit最佳实践融合现有规范 |

---

## 📎 相关文档

- **开发规范** - `.claude/core/RULES.md`
- **工作流程** - `.claude/core/SPEC-WORKFLOW.md`
- **架构设计** - `docs/architecture/README.md`
- **项目愿景** - `.spec-workflow/steering/product.md`
- **技术决策** - `.spec-workflow/steering/tech.md`

---

**最后更新**: 2025-10-16
**维护责任人**: 项目架构师
**审查周期**: 每季度一次