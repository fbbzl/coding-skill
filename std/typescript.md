# TypeScript 编码标准

技术实现核心三要素: 简单、合适、可扩展

优先级: 可读性 > 正确性 > 扩展性 > 性能

## 通用原则

0. 使用 TypeScript 5.x + Node.js 22+ 新特性
1. 不允许在循环中进行任何 TCP 通信
2. 造轮子是一件非常谨慎的事情，在造轮子之前请一定要确认 npm 生态中无该实现
3. 开发环境对安全的要求比较低，不要提前对安全性进行优化
4. 所有代码能对齐的尽量对齐（prettier），对齐可以极大提升可读性
5. 单个函数体不允许超过 40 行
6. 在不破坏语义的情况下，尽量使用 named import

## 命名与风格

0. 类名 / 接口名 / 类型名 PascalCase，函数和变量名 camelCase，常量 UPPER_SNAKE_CASE
1. 禁止使用 any；类型注解必须齐全（strict 模式）
2. 变量声明优先 const，其次 let，禁止 var
3. 所有可能返回 null / undefined 的方法签名中必须注明 `T | null` / `T | undefined`
4. 布尔变量/函数以 is/has/can/should 开头
5. 接口不加 I 前缀，类型别名用于联合 / 交叉类型

## 工具链

0. 校验使用 class-validator / zod，错误信息统一配置
1. 工具函数优先使用 lodash-es / native Node 模块，非必要不引入重型框架
2. 对象映射使用 class-transformer 或手动 mapper 函数
3. 格式化使用 Prettier，Lint 使用 ESLint + typescript-eslint
4. 包管理统一使用 pnpm

## 分层与职责

0. 基础校验放在 controller/route 层，业务校验放在 service 层
1. Service 方法需加简短精准的 JSDoc 注释
2. Service 层尽量不要跨其他 service 直接操作其持久层
3. 禁止硬编码配置类，业务配置通过 ConfigService 或静态引用暴露
4. @Controller / @Injectable 等装饰器类名尽量反映其职责

## 异常处理

0. 业务异常使用自定义 Error 子类，配合错误码常量
1. 异步代码必须 try/catch 或使用 .catch()，不允许 unhandled rejection
2. 异常信息必须包含足够上下文
3. 使用 neverthrow 或类似库处理可预期错误（可选）
4. 全局异常过滤器统一处理未捕获异常

## 数据库

0. 表字段设计要符合三范式
1. 字符集使用 utf8mb4（MySQL）或等效主流设置
2. 每张表除主键外尽量建索引，能创建联合唯一索引更好，需加唯一约束的加，包括联合唯一约束
3. 对数据库实体的更改要及时同步到 schema、dto、文档和 migration
4. 如果只需要获取 id，只查询 id 字段，不查全部记录再取 id
5. 注意数据库事务的使用（Prisma $transaction / TypeORM transaction / Sequelize transaction）
6. 禁止在循环中执行 SQL
7. 使用 ORM 的 createMany / bulkCreate 进行批量操作

## 测试

0. 使用 Vitest 或 Jest 作为测试框架
1. 测试函数名使用 should 期望 when 条件 格式
2. 使用 describe/it 组织测试用例
3. Mock 外部依赖，使用 vi.mock / jest.mock

## 日志

0. 使用 pino 或 winston 结构化日志
1. 入口方法记录入参，异常路径记录完整上下文
2. 禁止在循环中打印日志
3. 敏感信息脱敏后再打印
4. 禁止使用 console.log 作为正式日志（仅限调试）
