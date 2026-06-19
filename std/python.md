# Python 编码标准

技术实现核心三要素: 简单、合适、可扩展

优先级: 可读性 > 正确性 > 扩展性 > 性能

## 通用原则

0. 使用 Python 3.10+ 新特性：match/case、类型联合 `X | Y`、ParamSpec、TypeAlias、StrEnum 等
1. 不允许在循环中进行任何 TCP 通信
2. 造轮子是一件非常谨慎的事情，在造轮子之前请一定要确认生态中无该实现
3. 开发环境对安全的要求比较低，不要提前对安全性进行优化
4. 所有代码能对齐的尽量对齐，对齐可以极大提升可读性
5. 单个函数体不允许超过 40 行
6. 在不破坏语义的情况下，尽量使用 `from x import y` 精确导入

## 命名与风格

0. 类名 PascalCase，函数和变量名 snake_case，常量 UPPER_SNAKE_CASE
1. 禁止用单一字母或无意义命名
2. 类型注解必须齐全，所有公开函数签名都要有完整类型标注
3. 所有可能返回 None 的方法，签名中必须注明 `-> T | None`
4. 布尔变量/函数以 is_/has_/can_/should_ 开头
5. 模块级私有函数以单下划线 `_` 开头

## 工具链

0. 使用 dataclasses 或 Pydantic 减少样板代码，参考已有模块风格
1. 工具函数优先使用标准库，其次 httpx / aiofiles 等生态包，非必要不引入重型框架
2. 校验使用 Pydantic / marshmallow，错误信息统一配置或内联
3. 属性映射 / 序列化使用 Pydantic model_dump / dataclasses-json
4. 格式化使用 Ruff，类型检查使用 mypy 或 pyright

## 分层与职责

0. 基础校验放在视图层（route/handler），业务校验放在 Service 层
1. Service 方法需加简短精准的 docstring
2. Service 层尽量不要跨其他 Service 直接操作其持久层
3. 禁止硬编码配置类，业务配置通过模块级常量或 Settings 模型暴露
4. handler 函数名尽量反映其路由含义

## 异常处理

0. 业务异常使用自定义 Exception 子类，配合 ErrorCode 枚举
1. 不要裸 `except:`，至少 `except Exception:`
2. 异常信息必须包含足够上下文
3. 使用 contextlib.suppress 替代空 try/except 块
4. 异步代码注意 asyncio.CancelledError 的传播

## 数据库

0. 表字段设计要符合三范式
1. 字符集使用 utf8mb4（MySQL）或等效主流设置
2. 每张表除主键外尽量建索引，能创建联合唯一索引更好，需加唯一约束的加，包括联合唯一约束
3. 对数据库实体的更改要及时同步到 schema、dto/response_model、文档和 DDL
4. 如果只需要获取 id，只查询 id 字段，不查全部记录再取 id
5. 注意数据库事务的使用（SQLAlchemy session.commit/rollback 或 Django transaction.atomic）
6. 禁止在循环中执行 SQL
7. 使用 ORM 的 bulk_create/bulk_update 进行批量操作

## 测试

0. 使用 pytest 作为测试框架
1. 测试函数名使用 test_should_期望_when_条件 格式
2. 使用 fixture 管理测试状态，避免 setUp/tearDown
3. Mock 外部依赖，使用 pytest-mock 或 unittest.mock

## 日志

0. 使用 logging 模块，通过 `logger = logging.getLogger(__name__)` 获取
1. 使用延迟格式化 `logger.info("msg: %s", val)`，不使用 f-string
2. 禁止在循环中打印日志
3. 敏感信息脱敏后再打印

## Python 特色最佳实践

0. 使用 pathlib 替代 os.path 处理路径
1. 使用 context managers（`with`）管理资源
2. 用 dataclasses / pydantic 替代手写 __init__
3. 优先使用 list/dict comprehensions，但不超过两层
4. 异步代码统一使用 asyncio，不混用同步阻塞库

## 常见陷阱

0. 可变默认参数：`def f(x=[])` 会共享同一个列表
1. 在循环中修改正在遍历的 dict/list
2. 用 `==` 比较浮点数，金额用 Decimal
3. 全局 import * 污染命名空间
4. 忽略 `asyncio.gather` 中异常的传播
