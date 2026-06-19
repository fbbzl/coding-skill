# FastAPI 标准

## 项目结构

0. 按模块组织：`api/`、`services/`、`models/`、`schemas/`、`core/`、`db/`
1. 入口 `main.py` 只负责组装路由和中间件
2. 配置放在 `core/config.py`
3. 依赖注入使用 FastAPI 的 Depends

## 路由

0. 使用 APIRouter 按模块拆分
1. 路由前缀反映资源版本：`/api/v1/users`
2. 路径参数类型注解完整
3. 查询参数使用 Pydantic 模型或 Query 依赖

## 依赖注入

0. 数据库会话使用 Depends(get_db)
1. 当前用户依赖 get_current_user
2. 权限依赖 get_current_active_user
3. 依赖函数职责单一

## Schema

0. 使用 Pydantic BaseModel 定义请求/响应
1. 输入模型和输出模型分离
2. 字段必填/可选明确标注
3. 使用 Field 添加描述和校验
4. 枚举类型使用 Python Enum

## Service 层

0. 业务逻辑集中在 Service 函数中
1. 一个 Service 不直接操作其他模块的 Model
2. 数据库事务显式处理
3. 函数加简短精准注释

## 数据库

0. 使用 SQLAlchemy 2.0 风格
1. 模型声明使用 DeclarativeBase
2. 复杂查询使用 select() + join()
3. 异步使用 AsyncSession
4. 连接池参数按环境配置

## 异常

0. 定义 HTTPException 子类或自定义异常
1. 全局异常处理器统一返回错误格式
2. 错误码统一定义
3. 异常信息包含上下文

## 测试

0. 使用 pytest + TestClient
1. 数据库使用 SQLite 内存或测试库
2. 依赖项可以被 override
3. Mock 外部服务

## 异步

0. 路由函数默认 async
1. 数据库操作使用异步驱动
2. 外部 HTTP 调用使用 httpx.AsyncClient
3. 注意并发安全和连接泄漏

## 文档

0. 自动生成的 OpenAPI 文档保持准确
2. 复杂接口补充 description
3. 响应示例使用 response_model
