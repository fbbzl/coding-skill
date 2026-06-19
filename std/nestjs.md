# NestJS 标准

## 项目结构

0. 按模块组织：`src/modules/`、`src/common/`、`src/config/`、`src/database/`
1. 每个模块包含 module、controller、service、dto、entities
2. 全局模块注册核心依赖
3. 配置使用 @nestjs/config

## Module

0. 每个业务域一个 Module
1. Module 只导出必要的 Provider
2. 避免 Module 间循环依赖
3. 使用 forwardRef 解决必要循环依赖

## Controller

0. 使用 @Controller('resource') 声明路由前缀
1. HTTP 方法装饰器表达动作
2. 参数校验使用 DTO + ValidationPipe
3. 响应使用统一格式

## Service

0. 业务逻辑在 Service 中实现
1. 数据库操作通过 Repository 或 TypeORM
2. 事务使用 @Transactional() 或 QueryRunner
3. 方法加简短精准注释

## DTO

0. 使用 class-validator 校验
1. 请求 DTO 和响应 DTO 分离
2. 使用 PartialType、OmitType、PickType 减少样板
3. Swagger 装饰器补充文档

## 数据库

0. 使用 TypeORM 或 Prisma
1. Entity 与数据库表一一对应
2. 复杂查询使用 QueryBuilder
3. 索引在 Entity 中声明
4. 迁移使用 TypeORM Migration 或 Prisma Migrate

## 异常

0. 使用 HttpException 子类
1. 全局异常过滤器统一处理
2. 错误码统一定义
3. 异常响应格式一致

## 测试

0. 使用 Jest
1. Controller 测试 mock Service
2. Service 测试 mock Repository
3. E2E 测试使用 supertest + 测试数据库

## 微服务

0. 使用 @nestjs/microservices 时协议明确
1. 消息模式与事件模式区分清晰
2. 消息 DTO 统一
3. 失败重试和死信队列策略明确

## 部署

0. Docker 多阶段构建
1. 环境变量注入配置
2. 健康检查端点
3. PM2 或容器编排运行
