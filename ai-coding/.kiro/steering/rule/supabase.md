---
inclusion: fileMatch
fileMatchPattern: "*.py"
---
## Supabase 代码规范

### 1. 客户端初始化
- 在 `supabase/client.py` 中统一初始化客户端
- 客户端配置从环境变量获取
- 使用单例模式或依赖注入管理客户端实例
- 示例：
  ```python
  from supabase import create_client, Client
  from app.config import settings

  _supabase_client: Client | None = None

  def get_supabase_client() -> Client:
      """获取Supabase客户端实例（单例）"""
      global _supabase_client
      if _supabase_client is None:
          _supabase_client = create_client(
              settings.supabase_url,
              settings.supabase_key
          )
      return _supabase_client
  ```

### 2. 表操作规范
- 为每个表创建对应的操作类或函数集合
- 表名使用小写蛇形命名，与代码中的函数名对应
- 所有数据库操作必须包含错误处理

### 3. 查询规范
- 使用参数化查询，避免SQL注入风险
- 复杂查询使用视图（View）简化
- 限制返回字段，只获取需要的数据
- 分页查询必须包含 `limit` 和 `offset`
- 示例：
  ```python
  def get_users(page: int = 1, page_size: int = 20) -> list[dict]:
      """获取用户列表（分页）"""
      supabase = get_supabase_client()
      offset = (page - 1) * page_size
      
      response = (
          supabase.table("users")
          .select("id, username, email", count="exact")
          .order("created_at", desc=True)
          .range(offset, offset + page_size - 1)
          .execute()
      )
      
      if response.error:
          logger.error(f"查询用户失败: {response.error.message}")
          raise DatabaseError(f"获取用户列表失败: {response.error.message}")
      
      return {
          "data": response.data,
          "total": response.count,
          "page": page,
          "page_size": page_size
      }
  ```

### 4. 认证操作
- 封装认证相关操作（注册、登录、密码重置）
- 敏感认证信息不存储在前端状态
- JWT 令牌管理遵循安全最佳实践

### 5. 实时订阅
- 实时订阅集中管理，单独封装，避免全局订阅混乱重复订阅
- 订阅前检查用户权限
- 不需要时及时取消订阅，防止内存泄漏
- 订阅回调函数保持简洁，复杂逻辑另起函数处理
### 6. 权限控制
- 遵循最小权限原则设计数据库策略
- 敏感操作必须验证用户权限
- 前端传递的用户ID必须与认证信息验证

### 7. 数据验证
- 数据库操作前验证输入数据
- 使用 Pydantic 模型定义数据结构
- 示例：
  ```python
  from pydantic import BaseModel, EmailStr, field_validator

  class UserCreate(BaseModel):
      username: str
      email: EmailStr
      password: str
      
      @field_validator('password')
      def password_must_be_strong(cls, v):
          if len(v) < 8:
              raise ValueError('密码长度必须至少8个字符')
          return v
  ```
### 8.数据操作规范
- 所有数据库操作封装在 supabase/queries.py 中
- 按表或功能模块组织查询函数
- 复杂查询使用视图（View）简化
- 所有查询需处理可能的异常（网络错误、权限错误等）
### 9.安全规范
- 使用 Row Level Security (RLS) 保护数据访问
- 不在客户端执行敏感操作，通过 Supabase Edge Functions 处理
- 定期轮换服务密钥
- 所有用户输入在服务器端验证后再提交到数据库
### 10.认证集成
- 使用 Supabase Auth 进行用户认证
- 认证状态与 Gradio 状态同步
- 实现自动令牌刷新机制
- 保护需要登录的界面和 API