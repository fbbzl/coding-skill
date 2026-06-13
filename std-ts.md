# coding-skill / TypeScript

技术实现核心三要素: 简单合适可扩展, 不允许在循环中进行任何tcp通信

0 使用 TypeScript 5.x + Node.js 22+ 新特性，代码可读性 > 正确性 > 扩展性 > 性能
1 使用 class-validator / zod 或装饰器减少样板代码，参考已有模块风格
2 校验使用 class-validator / zod，错误信息统一配置
3 工具函数优先使用 lodash / native Node 模块，非必要不引入重型框架
4 对象映射使用 class-transformer 或手动 mapper 函数
5 基础校验放在 controller/route 层，业务校验放在 service 层。service 方法需加简短精准的注释
6 注意数据库事务的使用（Prisma $transaction / TypeORM transaction / Sequelize transaction）
7 业务异常使用自定义 Error 子类，配合错误码常量
8 @Controller / @Injectable 等装饰器类名尽量反映其职责
9 禁止使用 any 滥用；变量声明优先 const，其次 let，避免 var。类型注解必须齐全（strict 模式）
10 禁止硬编码配置类，业务配置通过 ConfigService 或静态引用暴露
11 在不破坏语义的情况下，尽量使用 named import
12 造轮子是一件非常谨慎的事情！在造轮子之前请一定要确认 npm 生态中无该实现
13 所有可能返回 null / undefined 的方法签名中必须注明 `T | null` / `T | undefined` 或使用 Optional 封装
14 所有代码能对齐的尽量对齐（prettier），对齐可以极大提升可读性
15 开发环境对安全的要求比较低，不要提前对安全性进行优化
16 service 层尽量不要跨其他 service 直接操作其持久层
17 单个函数体不允许超过 40 行

数据库
0 表字段设计要符合三范式
1 字符集使用 utf8mb4（MySQL）或等效主流设置
2 每张表除主键外尽量建索引，能创建联合唯一索引更好，需加唯一约束的加，包括联合唯一约束
3 对数据库实体的更改要及时同步到 schema、dto、文档和 migration
4 如果只需要获取 id，只查询 id 字段，不查全部记录再取 id
