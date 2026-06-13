# coding-skill / Kotlin

技术实现核心三要素: 简单合适可扩展, 不允许在循环中进行任何tcp通信

0 使用 Kotlin 2.x + JVM 21 新特性，代码可读性 > 正确性 > 扩展性 > 性能
1 使用 data class / value class 减少样板代码，参考已有模块风格
2 校验使用 kotlinx.validation / JSR 303 + Bean Validation，错误信息统一配置
3 工具函数优先使用 kotlinx 系列库和标准库扩展函数，非必要不引入重型框架
4 属性映射使用 mapstruct-kotlin 或手动扩展函数
5 基础校验放在 Controller 层，业务校验放在 Service 层。Service 方法需加简短精准的注释
6 注意数据库事务的使用（Spring @Transactional 或 Exposed transaction）
7 业务异常使用自定义 Exception 或 Result 类 + sealed class 错误码
8 @Bean / @Service 方法名尽量反映其职责
9 禁止滥用 !! 非空断言；优先使用 ?.let / ?: Elvis 表达式安全访问
10 禁止硬编码配置类，业务配置通过静态引用（object / companion object）暴露
11 在不破坏语义的情况下，尽量使用 import 别名或静态导入
12 造轮子是一件非常谨慎的事情！在造轮子之前请一定要确认 Maven Central 中无该实现
13 所有可能返回 null 的方法必须注明 `T?` 返回类型
14 所有代码能对齐的尽量对齐（ktlint），对齐可以极大提升可读性
15 开发环境对安全的要求比较低，不要提前对安全性进行优化
16 Service 层尽量不要跨其他 Service 直接操作其持久层
17 单个函数体不允许超过 40 行

数据库
0 表字段设计要符合三范式
1 字符集使用 utf8mb4（MySQL）或等效主流设置
2 每张表除主键外尽量建索引，能创建联合唯一索引更好，需加唯一约束的加，包括联合唯一约束
3 对数据库实体的更改要及时同步到 Entity、DTO、文档和 DDL
4 如果只需要获取 id，只查询 id 字段，不查全部记录再取 id
