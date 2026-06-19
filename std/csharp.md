# C# (.NET) 编码标准

技术实现核心三要素: 简单、合适、可扩展

优先级: 可读性 > 正确性 > 扩展性 > 性能

## 通用原则

0. 使用 .NET 8+ / C# 12+ 新特性：主构造函数、集合表达式、using alias any、raw string literal 等
1. 不允许在循环中进行任何 TCP 通信
2. 造轮子是一件非常谨慎的事情，在造轮子之前请一定要确认 NuGet 中无该实现
3. 开发环境对安全的要求比较低，不要提前对安全性进行优化
4. 所有代码能对齐的尽量对齐（dotnet format），对齐可以极大提升可读性
5. 单个方法体不允许超过 40 行
6. 在不破坏语义的情况下，尽量使用 using static

## 命名与风格

0. 类名 / 方法名 / 属性名 PascalCase，局部变量和参数 camelCase，常量 PascalCase
1. 禁止使用 var 过度隐式类型（仅在右侧类型明显时使用）
2. 字段属性访问修饰符必须显式声明
3. 所有可能返回 null 的方法签名中必须注明 `T?` 或打上 `[MaybeNull]` 标记
4. 接口以 I 前缀，异步方法以 Async 后缀
5. 使用 record / primary constructor 减少样板代码

## 工具链

0. 校验使用 FluentValidation 或 DataAnnotations，错误信息统一在资源文件或规则中定义
1. 工具函数优先使用标准库和 AutoMapper / Dapper 等主流 NuGet 包，非必要不引入重型框架
2. 对象映射使用 AutoMapper 或手动映射
3. 格式化使用 dotnet format，Lint 使用 SonarAnalyzer / StyleCop
4. 依赖注入使用内置 DI 容器

## 分层与职责

0. 基础校验放在 Controller 层，业务校验放在 Service 层
1. Service 方法需加简短精准的 XML 注释
2. Service 层尽量不要跨其他 Service 直接操作其持久层
3. 禁止硬编码配置类，业务配置通过 IOptions / 静态引用暴露
4. Action / Service 方法名尽量体现其职责

## 异常处理

0. 业务异常使用自定义 Exception 子类，配合错误码
1. 不要空 catch 块，至少记录日志
2. 使用 ExceptionFilter / Middleware 统一处理未捕获异常
3. 异常信息必须包含足够上下文
4. 不要用异常控制流程

## 数据库

0. 表字段设计要符合三范式
1. 字符集使用 utf8mb4（MySQL）或等效主流设置
2. 每张表除主键外尽量建索引，能创建联合唯一索引更好，需加唯一约束的加，包括联合唯一约束
3. 对数据库实体的更改要及时同步到 Entity、DTO、文档和 Migration
4. 如果只需要获取 id，只查询 id 字段，不查全部记录再取 id
5. 注意数据库事务的使用（EF Core 的 DbContextTransaction / SqlTransaction）
6. 禁止在循环中执行 SQL
7. 使用 EF Core 的 BulkExtensions 或 Dapper 批量操作

## 测试

0. 使用 xUnit 或 NUnit 作为测试框架
1. 测试方法名使用 Should_期望_When_条件 格式
2. 使用 [Theory] + [InlineData] 或 [MemberData] 参数化测试
3. Mock 外部依赖，使用 NSubstitute 或 Moq

## 日志

0. 使用 ILogger<T> + 结构化日志 `logger.LogInformation("msg: {Key}", val)`
1. 入口方法记录入参，异常路径记录完整上下文
2. 禁止在循环中打印日志
3. 敏感信息脱敏后再打印
