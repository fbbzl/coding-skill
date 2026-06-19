# API 设计标准

## RESTful 设计

0. 资源作为 URL 主体，使用名词复数：`/users`、`/orders`
1. 动作通过 HTTP Method 表达：GET / POST / PUT / PATCH / DELETE
2. URL 层级表达关联关系：`/users/{id}/orders`
3. 不使用动词作为 URL：`/getUser` 错误，`GET /users/{id}` 正确
4. 查询参数用于过滤、排序、分页：`?status=active&sort=-created_at&page=2&size=20`

## 状态码

0. 200 OK：成功
1. 201 Created：创建成功
2. 204 No Content：成功但无返回体
3. 400 Bad Request：请求参数错误
4. 401 Unauthorized：未认证
5. 403 Forbidden：无权限
6. 404 Not Found：资源不存在
7. 409 Conflict：业务冲突
8. 422 Unprocessable Entity：语义错误
9. 429 Too Many Requests：限流
10. 500 Internal Server Error：服务端错误
11. 503 Service Unavailable：服务不可用

## 请求/响应规范

0. 统一响应体格式：
   ```json
   {
     "code": 0,
     "message": "success",
     "data": {}
   }
   ```
1. 错误响应必须包含 code、message，可选 details
2. 时间使用 ISO 8601 格式：`2024-01-01T12:00:00Z`
3. 金额使用整数分/厘，避免浮点数
4. 枚举返回字符串而非数字
5. 批量操作使用数组接收

## 幂等性

0. GET、PUT、DELETE 必须幂等
1. POST 创建资源时，通过客户端生成唯一键或 Idempotency-Key 保证幂等
2. 重试机制必须配合幂等设计

## 版本控制

0. API 版本通过 URL 路径表达：`/v1/users`
1. 不兼容变更必须升级大版本
2. 废弃接口必须保留至少两个版本周期
3. 文档中标注每个接口的版本和废弃计划

## 认证与授权

0. 使用 OAuth 2.0 / JWT 进行认证
1. Token 必须设置过期时间
2. 敏感接口必须二次校验权限
3. 接口必须按最小权限原则设计
4. API Key 禁止前端硬编码

## 限流与降级

0. 公共接口必须配置限流
1. 限流按用户 + 接口维度组合
2. 核心链路必须有降级方案
3. 超时时间必须显式配置

## 文档

0. 每个接口必须包含用途、请求字段、响应字段、权限、幂等性、错误码
1. 使用 OpenAPI / Swagger 维护文档
2. 文档与代码同步更新
3. 提供 Mock 数据
