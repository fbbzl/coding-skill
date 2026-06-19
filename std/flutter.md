# Flutter 标准

## 项目结构

0. 按功能域组织：`lib/features/`、`lib/core/`、`lib/shared/`、`lib/data/`、`lib/domain/`
1. 入口 `main.dart`
2. 主题和路由配置放在 `core/`
3. 公共 Widget 放在 `shared/widgets/`

## Widget

0. 使用 StatelessWidget 优先
1. StatefulWidget 状态尽量局部化
2. Widget 类名 PascalCase，文件与类名一致
3. 拆分大 Widget 为小组件
4. const 构造函数优先

## 状态管理

0. 简单状态使用 StatefulWidget / ValueNotifier
1. 跨页面状态使用 Riverpod / Bloc / Provider
2. 业务逻辑与 UI 分离
3. 状态不可变更新

## 网络

0. HTTP 调用封装在 Repository/DataSource
1. 使用 Dio 并配置拦截器
2. 响应模型使用 freezed/json_serializable
3. 错误统一处理

## 导航

0. 使用 GoRouter 声明式路由
2. 路由参数类型安全
3. 页面跳转带上下文数据

## 本地存储

0. 简单键值使用 shared_preferences
1. 结构化数据使用 Hive / sqflite
2. 敏感数据加密存储

## UI/UX

0. 使用 Material 3 或 Cupertino 风格统一
1. 主题色、字体、间距使用 ThemeData
2. 响应式适配不同屏幕尺寸
3. 空状态、加载状态、错误状态处理完整
4. 支持深色模式

## 测试

0. 单元测试使用 flutter_test
1. Widget 测试测试交互
2. 集成测试覆盖核心流程
3. Mock 使用 mockito / mocktail

## 国际化

0. 使用 flutter_localizations + intl
1. 文本走 arb 文件
2. 日期、数字按 locale 格式化

## 性能

0. 图片缓存和压缩
1. 列表使用 ListView.builder
2. 避免在 build 中做重计算
3. 使用 const 减少重建
4. 启动时间优化
