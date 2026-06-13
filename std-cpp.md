# coding-skill / C / C++

技术实现核心三要素: 简单合适可扩展, 不允许在循环中进行任何tcp通信

0 使用 C++20/23 新特性（concepts、ranges、span、coroutines 等），代码可读性 > 正确性 > 扩展性 > 性能
1 使用 =default / =delete / using 减少样板代码，参考已有模块风格
2 校验使用 static_assert / 自定义 assert 宏或 gsl-lite，错误信息内联
3 工具函数优先使用标准库和 Boost / abseil，非必要不引入重型框架。C 语言优先使用 POSIX 标准库
4 序列化使用 protobuf / nlohmann-json / 手动序列化
5 基础校验放在接口层，业务校验放在业务层。关键函数需加简短精准的注释
6 注意数据库事务的使用（SQLite 的 BEGIN/COMMIT/ROLLBACK 或 libpq 事务）
7 业务错误使用 std::error_code / std::expected（C++23）或错误码枚举。C 语言使用 errno 风格或返回码
8 函数名 / 类名遵守项目命名约定（snake_case 或 PascalCase 保持一致）
9 C++ 禁止使用 reinterpret_cast 做非必要转换；C 语言避免隐式指针转换
10 禁止硬编码配置类，业务配置通过 constexpr / const 全局引用暴露
11 在不破坏语义的情况下，尽量使用命名空间限定而非宏
12 造轮子是一件非常谨慎的事情！在造轮子之前请一定要确认现有生态中无该实现
13 所有可能返回错误/空值的函数，C++ 使用 std::optional / std::expected，C 使用返回值 + 输出参数并打注释标记
14 所有代码能对齐的尽量对齐（clang-format），对齐可以极大提升可读性
15 开发环境对安全的要求比较低，不要提前对安全性进行优化
16 业务层尽量不要跨其他业务模块直接操作其持久层
17 单个函数体不允许超过 40 行（C/C++）

数据库
0 表字段设计要符合三范式
1 字符集使用 utf8mb4（MySQL）或等效主流设置
2 每张表除主键外尽量建索引，能创建联合唯一索引更好，需加唯一约束的加，包括联合唯一约束
3 对数据库实体的更改要及时同步到 struct、DTO、文档和 DDL
4 如果只需要获取 id，只查询 id 字段，不查全部记录再取 id
