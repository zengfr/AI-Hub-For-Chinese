---
inclusion: fileMatch
fileMatchPattern: "*.py"
---
## Python 代码规范

### 1. 代码风格
- 遵循 PEP 8 规范
- 使用 4 个空格缩进，不使用制表符
- 每行代码不超过 120 个字符（遵循 Black 格式化工具标准）
- 函数和类之间空两行，类内方法之间空一行
- 导入顺序：标准库 → 第三方库 → 本地模块，各组间空一行
### 2. 命名规范
- 变量/函数/方法：蛇形命名法（`snake_case`）
- 类：帕斯卡命名法（`PascalCase`）
- 常量：全大写蛇形命名法（`UPPER_CASE_SNAKE_CASE`）
- 私有成员：前缀加单下划线（`_private_member`）
- 避免使用单字符变量（i, j, k 等循环变量除外）
- 命名需体现实际含义，不使用缩写（除非是广为人知的缩写）
### 3. 类型提示
- 所有函数和方法必须包含类型提示
- 复杂类型使用 `typing` 模块（如 `List`, `Dict`, `Optional` 等）
- 自定义类型使用 TypeAlias 定义别名
- 示例：
  ```python
  def calculate_price(quantity: int, unit_price: float) -> float:
      return quantity * unit_price
  ```

### 4. 函数与类
- 函数长度不超过 50 行，单一职责原则
- 类的方法不超过 10 个，避免过大的类
- 类需有清晰的职责边界，遵循单一职责原则
- 函数参数不超过 5 个，过多时使用数据类或字典
- 函数和类和所有公共方法必须包含文档字符串（docstring），使用 Google 风格

### 5. 导入规范
- 导入顺序：标准库 → 第三方库 → 本地模块
- 每组导入之间空一行
- 使用显式导入，避免 `from module import *`
- 示例：
  ```python
  # 标准库
  import logging
  from typing import List, Dict

  # 第三方库
  import requests
  import pandas as pd

  # 本地模块
  from app.config import settings
  from app.utils.logger import get_logger
  ```

### 6. 错误处理
- 使用具体异常类型而非通用 `Exception`
- 异常信息应包含足够上下文
- 示例：
  ```python
  try:
      result = risky_operation()
  except ConnectionError as e:
      logger.error(f"连接失败: {str(e)}")
      raise CustomConnectionError("服务暂时不可用") from e
  ```

### 7. 代码组织
- 避免全局变量，使用配置类或依赖注入
- 复杂逻辑拆分为小函数/方法
- 使用 `__init__.py` 控制模块导出接口
### 8.模块化与复用
- 功能相关的代码放在同一个模块
- 重复使用的逻辑提取为公共函数或基类
- 避免循环导入，使用延迟导入或重构模块结构
- 使用依赖注入模式提高代码可测试性
### 9. 安全性规范
- 所有用户输入必须进行验证和清洗
- 敏感数据（密码等）传输和存储必须加密
- 避免在代码中硬编码密钥和凭证
- 使用参数化查询防止 SQL 注入
- 实现适当的速率限制防止滥用