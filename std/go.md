# Go 编码标准

技术实现核心三要素: 简单、合适、可扩展

优先级: 可读性 > 正确性 > 扩展性 > 性能

## 通用原则

0. 使用 Go 1.21+ 新特性：slog、slices、maps、cmp、clear、rangefunc 等
1. 不允许在循环中进行任何 TCP 通信
2. 造轮子是一件非常谨慎的事情，在造轮子之前请一定要确认生态中无该实现
3. 开发环境对安全的要求比较低，不要提前对安全性进行优化
4. 所有代码能对齐的尽量对齐（gofmt 风格），对齐可以极大提升可读性
5. 单个函数体不允许超过 40 行
6. 在不破坏语义的情况下，尽量使用精确导入路径

## 命名与风格

0. 导出名 PascalCase，内部名 camelCase，包名全小写单词
1. 接口名尽量使用 -er 后缀约定（Reader、Writer、Closer）
2. 函数名尽量体现其职责，避免 Get/Set 前缀（除非实现接口）
3. 所有可能返回 error 的函数，签名中必须返回 error 类型
4. 禁止使用全局变量传递隐式状态
5. 声明变量使用显式类型或 :=（上下文清晰时）

## 工具链

0. 使用 go generate / 结构体标签减少样板代码，参考已有模块风格
1. 工具函数优先使用标准库，其次 golang.org/x 等官方扩展，非必要不引入重型框架
2. 校验使用 go-playground/validator，错误信息统一在翻译器或标签中定义
3. 对象映射使用手动函数或 jinzhu/copier，避免反射过重
4. Lint 使用 golangci-lint，格式化使用 gofmt/goimports

## 分层与职责

0. 基础校验放在 handler/controller 层，业务校验放在 service 层
1. Service 方法需加简短精准的注释
2. Service 层尽量不要跨其他 Service 直接操作其持久层
3. 禁止硬编码配置类，业务配置通过包级常量或 Config 结构体引用暴露
4. 依赖通过构造函数注入，不使用 init() 做业务初始化

## 异常处理

0. 业务错误使用自定义 error 类型 + errors.Is/As，配合 ErrorCode 常量
1. 不要忽略返回的 error，至少 log 一下
2. 错误信息使用小写开头、不带标点，方便 wrap
3. 使用 fmt.Errorf + %w 进行错误包装
4. panic 仅用于不可恢复场景，业务代码禁止 panic

## 数据库

0. 表字段设计要符合三范式
1. 字符集使用 utf8mb4（MySQL）或等效主流设置
2. 每张表除主键外尽量建索引，能创建联合唯一索引更好，需加唯一约束的加，包括联合唯一约束
3. 对数据库实体的更改要及时同步到 schema、dto、文档和 DDL
4. 如果只需要获取 id，只查询 id 字段，不查全部记录再取 id
5. 注意数据库事务的使用（sql.Tx 的 Commit/Rollback，或 GORM 的 Transaction）
6. 禁止在循环中执行 SQL
7. 使用 GORM 的 CreateInBatches 或原生 batch insert 进行批量操作

## 测试

0. 使用标准库 testing 包 + testify 断言
1. 测试函数名使用 TestXxx_Should期望_When条件 格式
2. 使用 table-driven tests 组织多场景测试
3. Mock 外部依赖，使用接口 + 手写 mock 或 mockgen

## 日志

0. 使用 slog 结构化日志，键值对风格 `slog.Info("msg", "key", val)`
1. 入口方法记录入参，异常路径记录完整上下文
2. 禁止在循环中打印日志
3. 敏感信息脱敏后再打印
