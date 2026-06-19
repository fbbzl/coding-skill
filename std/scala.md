# Scala 编码标准

技术实现核心三要素: 简单、合适、可扩展

优先级: 可读性 > 正确性 > 扩展性 > 性能

## 通用原则

0. 使用 Scala 3.x 新特性：enum、union types、given/using、opaque type、extension methods 等
1. 不允许在循环中进行任何 TCP 通信
2. 造轮子是一件非常谨慎的事情，在造轮子之前请一定要确认 Maven Central 中无该实现
3. 开发环境对安全的要求比较低，不要提前对安全性进行优化
4. 所有代码能对齐的尽量对齐（scalafmt），对齐可以极大提升可读性
5. 单个方法体不允许超过 40 行
6. 在不破坏语义的情况下，尽量使用 import 通配或给定别名

## 命名与风格

0. 类名 / trait 名 PascalCase，方法名和变量名 camelCase，常量 PascalCase
1. 禁止使用 null；优先使用 Option / Either / Try 处理可空或可失败场景
2. 使用 case class / enum 减少样板代码
3. 所有可能返回空/错误的方法必须通过 Option / Either 或 Try 表达
4. 函数名尽量体现其纯函数性质
5. 隐式转换仅用于 type class 实例，不用于业务逻辑

## 工具链

0. 工具函数优先使用标准库和 cats / zio / circe 等生态包，非必要不引入重型框架
1. 序列化使用 circe / upickle / play-json，优先自动 derive
2. 校验使用 cats-validate / refined 类型或自定义 smart constructor
3. 格式化使用 scalafmt，Lint 使用 scalafix + wartremover
4. 构建使用 sbt 或 Mill

## 分层与职责

0. 基础校验放在 Controller / Route 层，业务校验放在 Service 层
1. Service 方法需加简短精准的注释
2. Service 层尽量不要跨其他 Service 直接操作其持久层
3. 禁止硬编码配置类，业务配置通过 object / given 实例引用暴露
4. 使用 tagless final 或 ZIO Layer 管理依赖

## 异常处理

0. 业务错误使用 ADT + sealed trait / enum，配合错误码
1. 使用 Either[Error, T] 或 IO/ZIO 的 typed error 通道
2. 不要抛异常控制流程，异常仅用于不可恢复场景
3. 使用 MonadError / ApplicativeError 组合错误处理
4. 异常信息必须包含足够上下文

## 数据库

0. 表字段设计要符合三范式
1. 字符集使用 utf8mb4（MySQL）或等效主流设置
2. 每张表除主键外尽量建索引，能创建联合唯一索引更好，需加唯一约束的加，包括联合唯一约束
3. 对数据库实体的更改要及时同步到 Entity、DTO、文档和 DDL
4. 如果只需要获取 id，只查询 id 字段，不查全部记录再取 id
5. 注意数据库事务的使用（Slick 的 transaction / doobie 的 transactor）
6. 禁止在循环中执行 SQL
7. 使用 batch insert 或 updateMany 进行批量操作

## 测试

0. 使用 ScalaTest 或 MUnit 作为测试框架
1. 测试用例名使用 `"should 期望 when 条件"` 格式
2. 使用 property-based testing（ScalaCheck）覆盖边界
3. Mock 外部依赖，使用 trait + 手工实现或 scalamock

## 日志

0. 使用 SLF4J + scala-logging `logger.info(s"msg: $val")`
1. 入口方法记录入参，异常路径记录完整上下文
2. 禁止在循环中打印日志
3. 敏感信息脱敏后再打印
