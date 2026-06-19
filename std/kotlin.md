# Kotlin 编码标准

技术实现核心三要素: 简单、合适、可扩展

优先级: 可读性 > 正确性 > 扩展性 > 性能

## 通用原则

0. 使用 Kotlin 2.x + JVM 21 新特性
1. 不允许在循环中进行任何 TCP 通信
2. 造轮子是一件非常谨慎的事情，在造轮子之前请一定要确认 Maven Central 中无该实现
3. 开发环境对安全的要求比较低，不要提前对安全性进行优化
4. 所有代码能对齐的尽量对齐（ktlint），对齐可以极大提升可读性
5. 单个函数体不允许超过 40 行
6. 在不破坏语义的情况下，尽量使用 import 别名或静态导入

## 命名与风格

0. 类名 PascalCase，函数和变量名 camelCase，常量 UPPER_SNAKE_CASE
1. 禁止滥用 !! 非空断言；优先使用 ?.let / ?: Elvis 表达式安全访问
2. 使用 data class / value class 减少样板代码
3. 所有可能返回 null 的方法必须注明 `T?` 返回类型
4. 优先使用 val 而非 var
5. 扩展函数只用于真正的工具行为，不要用来逃避依赖注入

## 工具链

0. 校验使用 kotlinx.validation / JSR 303 + Bean Validation，错误信息统一配置
1. 工具函数优先使用 kotlinx 系列库和标准库扩展函数，非必要不引入重型框架
2. 属性映射使用 mapstruct-kotlin 或手动扩展函数
3. 格式化使用 ktlint，Lint 使用 detekt
4. 协程使用 kotlinx.coroutines

## 分层与职责

0. 基础校验放在 Controller 层，业务校验放在 Service 层
1. Service 方法需加简短精准的注释
2. Service 层尽量不要跨其他 Service 直接操作其持久层
3. 禁止硬编码配置类，业务配置通过静态引用（object / companion object）暴露
4. @Bean / @Service 方法名尽量反映其职责

## 异常处理

0. 业务异常使用自定义 Exception 或 Result 类 + sealed class 错误码
1. 使用 runCatching / Result 处理可预期错误
2. 不要吞掉异常，至少记录日志
3. 协程中注意 CancellationException 的传播
4. 异常信息必须包含足够上下文

## 数据库

0. 表字段设计要符合三范式
1. 字符集使用 utf8mb4（MySQL）或等效主流设置
2. 每张表除主键外尽量建索引，能创建联合唯一索引更好，需加唯一约束的加，包括联合唯一约束
3. 对数据库实体的更改要及时同步到 Entity、DTO、文档和 DDL
4. 如果只需要获取 id，只查询 id 字段，不查全部记录再取 id
5. 注意数据库事务的使用（Spring @Transactional 或 Exposed transaction）
6. 禁止在循环中执行 SQL
7. 使用 saveAll / batchInsert 进行批量操作

## 测试

0. 使用 JUnit 5 + kotest 或 strikt 断言
1. 测试函数名使用反引号 `should 期望 when 条件` 格式
2. 使用 @ParameterizedTest 参数化测试
3. Mock 外部依赖，使用 MockK

## 日志

0. 使用 SLF4J + KotlinLogging `logger.info { "msg: $val" }`
1. 入口方法记录入参，异常路径记录完整上下文
2. 禁止在循环中打印日志
3. 敏感信息脱敏后再打印
