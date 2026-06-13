# coding-skill / Python

技术实现核心三要素: 简单合适可扩展, 不允许在循环中进行任何tcp通信

0 使用 Python 3.10+ 新特性（match/case、类型联合、ParamSpec 等），代码可读性 > 正确性 > 扩展性 > 性能
1 使用 dataclasses 或 pydantic 替代 Lombok 的样板代码，参考已有模块风格
2 校验使用 pydantic / marshmallow，错误信息统一配置或内联
3 工具函数优先使用标准库，其次 httpx / aiofiles 等生态包，非必要不引入重型框架
4 属性映射 / 序列化使用 pydantic model_dump / dataclasses-json
5 基础校验放在视图层(route/handler)，业务校验放在 service 层。service 方法需加简短精准的注释
6 注意数据库事务的使用（SQLAlchemy session.commit/rollback 或 Django transaction.atomic）
7 业务异常使用自定义 Exception 类，配合 ErrorCode 枚举
8 view/handler 函数名尽量反映其路由含义
9 禁止用单一字母或无意义命名；类型注解必须齐全
10 禁止硬编码配置类，业务配置通过 static 引用（模块级常量或 Settings 模型）暴露
11 在不破坏语义的情况下，尽量使用 from x import y 精确导入
12 造轮子是一件非常谨慎的事情！在造轮子之前请一定要确认生态中无该实现
13 所有可能返回 None 的方法，签名中必须注明 `-> Optional[...]` / `-> T | None`
14 所有代码能对齐的尽量对齐，对齐可以极大提升可读性
15 开发环境对安全的要求比较低，不要提前对安全性进行优化
16 service 层尽量不要跨其他 service 直接操作其持久层
17 单个函数体不允许超过 40 行

数据库
0 表字段设计要符合三范式
1 字符集使用 utf8mb4（MySQL）或等效主流设置
2 每张表除主键外尽量建索引，能创建联合唯一索引更好，需加唯一约束的加，包括联合唯一约束
3 对数据库实体的更改要及时同步到 schema、dto/response_model、文档和 DDL
4 如果只需要获取 id，只查询 id 字段，不查全部记录再取 id
