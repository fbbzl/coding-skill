# Angular 标准

## 项目结构

0. 按模块组织：`src/app/features/`、`src/app/shared/`、`src/app/core/`
1. CoreModule 只导入一次，提供单例服务
2. SharedModule 包含公共组件、指令、管道
3. FeatureModule 按需懒加载

## 组件

0. 组件名 PascalCase，选择器使用 kebab-case
1. 模板与样式就近放置
2. 输入属性使用 @Input()，输出使用 @Output()
3. OnPush 变更检测策略优先
4. 组件职责单一

## 服务

0. 业务逻辑放在 Injectable 服务中
1. HTTP 调用集中在 Service 层
2. 服务按领域拆分
3. 使用 BehaviorSubject 管理共享状态

## 模块

0. 每个 Feature 一个 Module
1. 公共组件/指令/管道放入 SharedModule
2. 核心服务放入 CoreModule
3. 避免 Module 间循环依赖

## 路由

0. 路由配置使用 Route 数组
1. 懒加载 Feature Module
2. 路由守卫处理认证和权限
3. 路由参数和查询参数类型安全

## 表单

0. 响应式表单优先于模板驱动表单
1. 校验器使用 Validators 或自定义校验器
2. 表单状态（pristine/touched/invalid）用于 UI 反馈
3. 表单提交状态处理完整

## 依赖注入

0. 使用构造函数注入
1. 提供服务范围按需：root / module / component
2. 使用 InjectionToken 管理非类依赖

## HTTP

0. 使用 HttpClient 并封装拦截器
1. 统一错误处理拦截器
2. Token 注入拦截器
3. Loading 状态通过拦截器或 Service 管理

## 测试

0. 使用 Jasmine + Karma 或 Jest
1. 组件测试使用 TestBed
2. Service 测试 mock 依赖
3. E2E 使用 Playwright 或 Cypress

## 变更检测

0. OnPush 减少重渲染
1. 使用 async pipe 订阅 Observable
2. 避免在模板中调用函数
3. 不可变数据更新
