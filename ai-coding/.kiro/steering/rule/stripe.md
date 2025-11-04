---
inclusion: fileMatch
fileMatchPattern: "*.py"
---
## Stripe 代码规范

### 1. 客户端初始化
- 在 `stripe/client.py` 中统一初始化客户端
- 根据环境（开发/生产）使用不同的API密钥
- 示例：
  ```python
  import stripe
  from app.config import settings

  # 初始化Stripe客户端
  stripe.api_key = settings.stripe_api_key
  
  # 设置API版本
  stripe.api_version = "2023-10-16"
  ```

### 2. 支付流程
- 支付流程拆分为明确的步骤函数：
  - 创建价格（create_price）
  - 创建支付意向（create_payment_intent）
  - 确认支付（confirm_payment）
  - 处理支付结果（handle_payment_result）
- 支付流程分为创建支付意向、处理支付结果、更新订单状态三个步骤
- 所有支付金额以最小货币单位（分 / 美分）处理
- 支付前验证商品信息和价格，防止客户端篡改
- 支付结果通过 Webhook 异步确认，不依赖客户端反馈
### 3. 事件处理
- 统一的Webhook处理函数,Webhook 端点单独实现，放在 stripe/webhooks.py
- 不同事件类型使用不同的处理方法
- 严格验证 Webhook 签名，防止伪造请求
- 处理 Webhook 时使用幂等性设计，避免重复处理
- 实现 Webhook 事件类型的分发机制


### 4. 错误处理
- 捕获并处理Stripe特定异常（stripe.error）
- 向用户返回友好的错误信息
- 记录详细错误日志用于调试
- 支付失败时提供用户友好的错误信息和解决建议
- 实现支付异常监控和告警机制

### 5. 数据转换
- 统一金额单位转换（Stripe使用分作为单位）
- 定义统一的数据模型转换函数
- 示例：
  ```python
  def convert_to_stripe_amount(amount: float, currency: str = "usd") -> int:
      """将金额转换为Stripe所需的最小单位（分）"""
      if currency in ["jpy", "krw"]:  # 这些货币没有小数位
          return int(amount)
      return int(amount * 100)
  ```

### 6. 安全规范
- 不在前端存储敏感支付信息,不在前端暴露 Stripe 密钥
- 所有支付操作通过后端代理
- PCI合规：不直接处理信用卡信息
- 使用 Stripe Elements 处理支付卡信息，避免敏感数据经过自己的服务器
- 实现防重复支付机制（幂等性）
- 定期审计支付记录，检查异常交易
### 7. 日志与监控
- 记录所有支付相关操作
- 关键事件（支付成功/失败）必须记录详细日志
- 实现支付状态监控和告警机制
### 8.定价管理
- 产品和价格信息优先从 Stripe Dashboard 管理
- 客户端只显示价格 ID，不硬编码价格金额
- 实现价格缓存机制，减少 API 调用
- 价格变动时通过 Webhook 同步到本地数据库
 
 