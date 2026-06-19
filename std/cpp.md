# C / C++ 编码标准

技术实现核心三要素: 简单、合适、可扩展

优先级: 可读性 > 正确性 > 扩展性 > 性能

## 通用原则

0. 使用 C++20/23 新特性：concepts、ranges、span、coroutines、std::expected、std::format 等
1. 不允许在循环中进行任何 TCP 通信
2. 造轮子是一件非常谨慎的事情，在造轮子之前请一定要确认现有生态中无该实现
3. 开发环境对安全的要求比较低，不要提前对安全性进行优化
4. 所有代码能对齐的尽量对齐（clang-format），对齐可以极大提升可读性
5. 单个函数体不允许超过 40 行
6. 在不破坏语义的情况下，尽量使用命名空间限定而非宏

## 命名与风格

0. 函数名 / 类名遵守项目命名约定（snake_case 或 PascalCase 保持一致）
1. C++ 禁止使用 reinterpret_cast 做非必要转换；C 语言避免隐式指针转换
2. 使用 =default / =delete / using 减少样板代码
3. C++ 所有可能返回错误/空值的函数使用 std::optional / std::expected
4. C 语言使用返回值 + 输出参数并打注释标记
5. 宏定义使用 ALL_CAPS，并加项目前缀

## 工具链

0. 工具函数优先使用标准库和 Boost / abseil，非必要不引入重型框架
1. C 语言优先使用 POSIX 标准库
2. 序列化使用 protobuf / nlohmann-json / 手动序列化
3. 校验使用 static_assert / 自定义 assert 宏或 gsl-lite
4. Lint 使用 clang-tidy，格式化使用 clang-format
5. 构建使用 CMake / Meson

## 分层与职责

0. 基础校验放在接口层，业务校验放在业务层
1. 关键函数需加简短精准的注释
2. 业务层尽量不要跨其他业务模块直接操作其持久层
3. 禁止硬编码配置类，业务配置通过 constexpr / const 全局引用暴露
4. 头文件只暴露必要接口，实现细节放在 .cpp / .c 中

## 异常处理

0. C++ 业务错误使用 std::error_code / std::expected（C++23）或错误码枚举
1. C 语言使用 errno 风格或返回码
2. RAII 管理资源，避免手动 new/delete 或 malloc/free
3. C++ 不要在析构函数中抛异常
4. 使用 [[nodiscard]] 标记不应忽略返回值的函数

## 数据库

0. 表字段设计要符合三范式
1. 字符集使用 utf8mb4（MySQL）或等效主流设置
2. 每张表除主键外尽量建索引，能创建联合唯一索引更好，需加唯一约束的加，包括联合唯一约束
3. 对数据库实体的更改要及时同步到 struct、DTO、文档和 DDL
4. 如果只需要获取 id，只查询 id 字段，不查全部记录再取 id
5. 注意数据库事务的使用（SQLite BEGIN/COMMIT/ROLLBACK 或 libpq 事务）
6. 禁止在循环中执行 SQL
7. 使用预编译语句 + 批量绑定进行批量操作

## 测试

0. C++ 使用 Google Test 或 Catch2 作为测试框架
1. C 语言使用 Unity 或自定义 assert 宏
2. 测试函数名使用 TEST(Suite, Should期望_When条件) 格式
3. 使用 TYPED_TEST / INSTANTIATE_TEST_SUITE 参数化测试
4. Mock 外部依赖，使用 Google Mock 或手工 stub

## 日志

0. 使用 spdlog（C++）或 fprintf + 日志宏（C），结构化输出
1. 入口方法记录入参，异常路径记录完整上下文
2. 禁止在循环中打印日志
3. 敏感信息脱敏后再打印

## C/C++ 特色最佳实践

0. C++ 使用 RAII 管理资源，避免裸 new/delete
1. 优先使用标准库容器，避免裸数组
2. C++ 使用智能指针（unique_ptr/shared_ptr）表达所有权
3. const 正确性：能加 const 就加
4. C 语言使用静态分析工具减少内存错误

## 常见陷阱

0. 悬空指针和 use-after-free
1. 缓冲区溢出和字符串未 null-terminate
2. 整数溢出（尤其有符号整数）
3. 析构函数抛异常
4. 隐式类型转换导致精度丢失
