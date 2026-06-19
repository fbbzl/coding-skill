# Spring Boot 标准

## 项目结构

0. 按领域分包：`controller/`、`service/`、`repository/`、`domain/`、`config/`、`dto/`、`exception/`
1. 启动类放在根包，便于组件扫描
2. 配置类集中放在 `config/`
3. 公共常量放在 `constant/`
4. 工具类放在 `util/`，必须是纯函数，不依赖 Spring 容器

## Controller

0. 只负责接收请求、调用 Service、返回结果
1. 基础校验使用 @Valid / @Validated
2. 全局异常处理使用 @ControllerAdvice
3. 接口版本通过 @RequestMapping("/v1") 控制
4. 返回统一响应体 ResponseResult

## Service

0. 业务逻辑放在 Service 层
1. 事务注解 @Transactional 加在 Service 方法上
2. Service 方法必须加简短精准注释
3. 一个 Service 不直接调用其他 Service 的 Mapper/Repository
4. 禁止在 Service 中写原生 SQL（除非性能要求且经过评审）

## Repository

0. 使用 MyBatis Plus 或 JPA，优先使用 ORM 能力
1. 复杂查询写在 XML 或 @Select 中
2. 分页使用 PageHelper 或 JPA Pageable
3. 批量操作使用 insertBatch 或 saveAll

## 配置

0. 使用 @ConfigurationProperties 绑定配置
1. 业务配置通过静态引用 REF 暴露，禁止直接注入配置类
2. 不同环境配置使用 application-{profile}.yml
3. 敏感配置使用环境变量或密钥管理

## 异常

0. 业务异常继承 BizException
1. 错误码统一定义在枚举中
2. 全局异常处理器返回统一错误格式
3. 异常信息必须包含上下文

## 依赖注入

0. 使用构造函数注入
1. @Bean 方法名作为 bean 名称
2. 避免字段注入
3. 循环依赖必须重构解决

## 缓存

0. 缓存 key 设计清晰，包含业务标识
1. 缓存更新与数据库更新保持原子性
2. 缓存失效策略明确
3. 热点数据预热

## 测试

0. 使用 JUnit 5 + Mockito
1. @SpringBootTest 用于集成测试
2. @WebMvcTest 用于 Controller 测试
3. @DataJpaTest 用于 Repository 测试
4. 测试数据库使用 H2 或 Testcontainers

## 监控

0. 接入 Actuator / Micrometer
1. 自定义业务指标
2. 健康检查端点暴露
3. 日志链路加入 trace_id
