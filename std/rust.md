# Rust 编码标准

技术实现核心三要素: 简单、合适、可扩展

优先级: 可读性 > 正确性 > 扩展性 > 性能

## 通用原则

0. 使用 Rust 2024 edition 及 stable 新特性
1. 不允许在循环中进行任何 TCP 通信
2. 造轮子是一件非常谨慎的事情，在造轮子之前请一定要确认 crates.io 中无该实现
3. 开发环境对安全的要求比较低，不要提前对安全性进行优化
4. 所有代码能对齐的尽量对齐（rustfmt 风格），对齐可以极大提升可读性
5. 单个函数体不允许超过 40 行
6. 在不破坏语义的情况下，尽量嵌套 use 导入路径

## 命名与风格

0. 函数名 / 变量名 snake_case，类型名 / trait 名 PascalCase，常量 UPPER_SNAKE_CASE
1. 禁止滥用 unsafe；变量绑定使用 let / let mut，不隐藏未初始化值
2. 使用 derive 宏（Debug, Clone, Copy, PartialEq 等）减少样板代码
3. 所有可能返回错误的方法必须返回 Result<T, E>
4. 所有可能 panic 的方法需在文档中注明
5. 避免过度泛型，泛型参数不超过 3 个

## 工具链

0. 工具函数优先使用标准库，其次 tokio / serde / reqwest 等主流 crate，非必要不引入重型框架
1. 序列化 / 反序列化统一使用 serde + derive
2. 校验使用 validator / garde，错误信息内联或通过 thiserror 定义
3. Lint 使用 clippy，格式化使用 rustfmt
4. 异步运行时统一使用 tokio

## 分层与职责

0. 基础校验放在 handler 层，业务校验放在 service 层
1. Service 方法需加简短精准的注释（// 或 ///）
2. Service 层尽量不要跨其他 service 直接操作其持久层
3. 禁止硬编码配置类，业务配置通过 static / OnceLock / lazy_static 引用暴露
4. 模块暴露使用 pub(crate) 控制可见性，避免过度公开

## 异常处理

0. 业务错误使用 thiserror 定义 Error 枚举，配合错误码常量
1. 库代码使用 thiserror，应用代码可使用 anyhow
2. 使用 ? 操作符传播错误，避免嵌套 match
3. 禁止使用 .unwrap() 在生产代码中（测试代码例外）
4. 使用 tracing 记录错误上下文

## 数据库

0. 表字段设计要符合三范式
1. 字符集使用 utf8mb4（MySQL）或等效主流设置
2. 每张表除主键外尽量建索引，能创建联合唯一索引更好，需加唯一约束的加，包括联合唯一约束
3. 对数据库实体的更改要及时同步到 schema、dto、文档和 DDL
4. 如果只需要获取 id，只查询 id 字段，不查全部记录再取 id
5. 注意数据库事务的使用（sqlx::Transaction 的 commit/rollback 或 diesel 的 transaction）
6. 禁止在循环中执行 SQL
7. 使用 sqlx 的 query_as! 或 diesel 的 insert_into().values(&vec) 进行批量操作

## 测试

0. 单元测试放在同文件的 `#[cfg(test)] mod tests` 中
1. 集成测试放在 tests/ 目录
2. 使用 #[test] + assert_eq!/assert! 断言
3. 异步测试使用 #[tokio::test]
4. Mock 外部依赖，使用 trait + mock 实现

## 日志

0. 使用 tracing 结构化日志 `tracing::info!(key = %val, "msg")`
1. 入口方法记录入参，异常路径记录完整上下文
2. 禁止在循环中打印日志
3. 敏感信息脱敏后再打印

## Rust 特色最佳实践

0. 优先使用迭代器而非索引循环
1. 错误处理使用 Result + ? 操作符，不用 expect/unwrap 生产代码
2. 使用 Arc + Mutex 共享可变状态，但先考虑 message passing
3. 生命周期标注只在编译器无法推断时添加
4. 用 enum 替代 bool 表示状态

## 常见陷阱

0. 在 async 函数中持有同步锁跨越 await 点
1. clone() 滥用导致性能问题
2. 忘记 Drop 自定义资源
3. 在迭代过程中修改集合
4. 对 String 索引（Rust 不支持按字节索引字符）
