# coding-skill / C# (.NET)

技术实现核心三要素: 简单合适可扩展, 不允许在循环中进行任何tcp通信

0 使用 .NET 8+ / C# 12+ 新特性（主构造函数、集合表达式、using alias any 等），代码可读性 > 正确性 > 扩展性 > 性能
1 使用 record / primary constructor 减少样板代码，参考已有模块风格
2 校验使用 FluentValidation 或 DataAnnotations，错误信息统一在资源文件或规则中定义
3 工具函数优先使用标准库和 FluentValidation / AutoMapper / Dapper 等主流 NuGet 包，非必要不引入重型框架
4 对象映射使用 AutoMapper 或手动映射
5 基础校验放在 Controller 层，业务校验放在 Service 层。Service 方法需加简短精准的 XML 注释
6 注意数据库事务的使用（EF Core 的 DbContextTransaction / SqlTransaction）
7 业务异常使用自定义 Exception 子类，配合错误码
8 Action / Service 方法名尽量体现其职责
9 禁止使用 var 过度隐式类型；字段属性访问修饰符必须显式声明
10 禁止硬编码配置类，业务配置通过 IOptions / 静态引用暴露
11 在不破坏语义的情况下，尽量使用 using static
12 造轮子是一件非常谨慎的事情！在造轮子之前请一定要确认 NuGet 中无该实现
13 所有可能返回 null 的方法签名中必须注明 `T?` / `Nullable<T>` 或打上 `[MaybeNull]` 标记
14 所有代码能对齐的尽量对齐（dotnet format），对齐可以极大提升可读性
15 开发环境对安全的要求比较低，不要提前对安全性进行优化
16 Service 层尽量不要跨其他 Service 直接操作其持久层
17 单个方法体不允许超过 40 行

数据库
0 表字段设计要符合三范式
1 字符集使用 utf8mb4（MySQL）或等效主流设置
2 每张表除主键外尽量建索引，能创建联合唯一索引更好，需加唯一约束的加，包括联合唯一约束
3 对数据库实体的更改要及时同步到 Entity、DTO、文档和 Migration
4 如果只需要获取 id，只查询 id 字段，不查全部记录再取 id
