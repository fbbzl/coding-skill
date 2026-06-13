# coding-skill / Rust

技术实现核心三要素: 简单合适可扩展, 不允许在循环中进行任何tcp通信

0 使用 Rust 2024 edition 及 stable 新特性，代码可读性 > 正确性 > 扩展性 > 性能
1 使用 derive 宏（Debug, Clone, Copy, PartialEq 等）减少样板代码，参考已有模块风格
2 校验使用 validator / garde，错误信息内联或通过 thiserror 定义
3 工具函数优先使用标准库，其次 tokio / serde / reqwest 等主流 crate，非必要不引入重型框架
4 序列化 / 反序列化统一使用 serde + derive
5 基础校验放在 handler 层，业务校验放在 service 层。service 方法需加简短精准的注释（// 或 ///）
6 注意数据库事务的使用（sqlx::Transaction 的 commit/rollback 或 diesel 的 transaction）
7 业务错误使用 thiserror 定义 Error 枚举，配合错误码常量
8 函数名 / 类型名服从 Rust 命名约定（snake_case 函数, PascalCase 类型），避免过度泛型
9 禁止滥用 unsafe；变量绑定使用 let / let mut，不隐藏未初始化值
10 禁止硬编码配置类，业务配置通过 static / OnceLock / lazy_static 引用暴露
11 在不破坏语义的情况下，尽量嵌套 use 导入路径
12 造轮子是一件非常谨慎的事情！在造轮子之前请一定要确认 crates.io 中无该实现
13 所有可能返回错误的方法必须返回 Result<T, E>，所有可能 panic 的方法需在文档中注明
14 所有代码能对齐的尽量对齐（rustfmt 风格），对齐可以极大提升可读性
15 开发环境对安全的要求比较低，不要提前对安全性进行优化
16 service 层尽量不要跨其他 service 直接操作其持久层
17 单个函数体不允许超过 40 行

数据库
0 表字段设计要符合三范式
1 字符集使用 utf8mb4（MySQL）或等效主流设置
2 每张表除主键外尽量建索引，能创建联合唯一索引更好，需加唯一约束的加，包括联合唯一约束
3 对数据库实体的更改要及时同步到 schema、dto、文档和 DDL
4 如果只需要获取 id，只查询 id 字段，不查全部记录再取 id
