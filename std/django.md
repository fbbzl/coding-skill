# Django 标准

## 项目结构

0. 按 App 拆分业务，每个 App 包含 models/views/serializers/services/tests
1. 公共工具放在 `common/` 或 `utils/`
2. 配置按环境拆分：`settings/base.py`、`settings/dev.py`、`settings/prod.py`
3. 入口统一使用 `manage.py`

## Models

0. 每个 Model 必须有 __str__
1. 使用 Meta 配置表名、索引、排序
2. 外键使用 related_name
3. 时间字段使用 auto_now_add / auto_now
4. 业务方法放在 Model 的 methods 中

## Views

0. 优先使用 Class-Based Views 或 DRF ViewSet
1. 基础校验放在 Serializer
2. 业务逻辑放在 Service 层
3. 权限校验使用 DRF permission classes
4. 响应使用统一格式

## Serializers

0. DRF Serializer 负责输入校验和输出序列化
1. validate_ 方法处理字段级校验
2. validate 方法处理对象级校验
3. 嵌套序列化器注意 N+1 问题

## 业务层

0. Service 函数接收已校验的参数
1. 一个 Service 不直接操作其他 App 的 Model
2. 数据库事务使用 `transaction.atomic`
3. 方法加简短精准注释

## URL 路由

0. 使用 path()，不使用 url()
1. App 路由 include 到项目路由
2. 命名空间清晰，便于 reverse

## 管理后台

0. 为业务 Model 注册 admin
1. 使用 list_display、search_fields、list_filter
2. 敏感操作加日志

## 测试

0. 使用 pytest-django
1. Model 测试、View 测试、Service 测试分离
2. 使用 factory_boy 创建测试数据
3. 测试数据库隔离

## 异步任务

0. 使用 Celery 处理异步任务
1. 任务函数幂等
2. 任务结果按需存储
3. 失败任务重试策略明确

## 部署

0. 使用 Gunicorn + Nginx 或 ASGI 服务器
1. 静态文件使用 collectstatic
2. 环境变量管理 SECRET_KEY 等敏感配置
3. 数据库迁移在部署前执行

## 常见陷阱

0. N+1 查询：使用 select_related / prefetch_related
1. 在信号量中做重逻辑影响性能
2. 模板中直接写数据库查询导致难以追踪
3. 未关闭 QuerySet 惰性求值导致意外查询
4. settings.SECRET_KEY 硬编码在代码中
5. 在生产环境开启 DEBUG
