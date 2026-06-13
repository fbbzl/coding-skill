# coding-skill / Scala

技术实现核心三要素: 简单合适可扩展, 不允许在循环中进行任何tcp通信

0 使用 Scala 3.x 新特性（enum、union types、given/using、opaque type 等），代码可读性 > 正确性 > 扩展性 > 性能
1 使用 case class / enum 减少样板代码，参考已有模块风格
2 校验使用 cats-validate / ScalaCheck 或自定义 smart constructor，错误信息内联
3 工具函数优先使用标准库和 cats / zio / circe 等生态包，非必要不引入重型框架
4 序列化使用 circe / upickle / play-json，优先自动 derive
5 基础校验放在 Controller / Route 层，业务校验放在 Service 层。Service 方法需加简短精准的注释
6 注意数据库事务的使用（Slick 的 transaction / doobie 的 transactor）
7 业务错误使用 ADT + sealed trait / enum，配合错误码
8 函数名尽量体现其纯函数性质，方法遵循驼峰命名
9 禁止使用 null；优先使用 Option / Either / Try 处理可空或可失败场景
10 禁止硬编码配置类，业务配置通过 object / given 实例引用暴露
11 在不破坏语义的情况下，尽量使用 import 通配或给定别名
12 造轮子是一件非常谨慎的事情！在造轮子之前请一定要确认 Maven Central 中无该实现
13 所有可能返回空/错误的方法必须通过 Option / Either 或 Try 表达
14 所有代码能对齐的尽量对齐（scalafmt），对齐可以极大提升可读性
15 开发环境对安全的要求比较低，不要提前对安全性进行优化
16 Service 层尽量不要跨其他 Service 直接操作其持久层
17 单个方法体不允许超过 40 行

数据库
0 表字段设计要符合三范式
1 字符集使用 utf8mb4（MySQL）或等效主流设置
2 每张表除主键外尽量建索引，能创建联合唯一索引更好，需加唯一约束的加，包括联合唯一约束
3 对数据库实体的更改要及时同步到 Entity、DTO、文档和 DDL
4 如果只需要获取 id，只查询 id 字段，不查全部记录再取 id
