# Vue 标准

## 项目结构

0. 按功能域组织：`components/`、`views/`、`composables/`、`stores/`、`services/`、`utils/`
1. 公共组件放在 `components/common/`
2. 页面放在 `views/` 或 `pages/`
3. 路由使用 Vue Router

## 组件

0. 使用 Composition API（<script setup>）
1. 组件名 PascalCase，文件与组件名一致
2. Props 使用 defineProps 并声明类型
3. Emits 使用 defineEmits
4. 列表渲染使用稳定 key

## Composables

0. 以 use 开头命名
1. 一个 composable 职责单一
2. 返回对象或数组，保持调用处清晰
3. 避免副作用污染全局状态

## 状态管理

0. 本地状态使用 ref / reactive
1. 跨组件状态使用 Pinia
2. 服务端状态使用 Vue Query / 自定义 fetch 封装
3. Store 按模块拆分

## 性能

0. 使用 v-once/v-memo 减少重渲染（必要时）
1. 大列表使用虚拟滚动
2. 组件异步加载
3. 图片懒加载
4. 避免深层响应式对象

## 样式

0. 使用 scoped 样式或 CSS Modules
1. 使用 Tailwind / UnoCSS 或统一的设计系统
2. 主题变量使用 CSS 变量
3. 响应式移动优先

## 表单

0. 使用 VeeValidate 或自定义表单校验
2. 提交状态处理完整
3. 受控组件优先

## 路由

0. 路由配置集中管理
1. 路由守卫处理权限
2. 路由懒加载
3. 路由 meta 信息补充权限/标题

## 测试

0. 使用 Vitest + Vue Test Utils
1. 测试组件交互而非实现细节
2. Mock Pinia 和 API 调用
3. E2E 使用 Playwright

## 类型

0. 使用 TypeScript + Vue 类型支持
1. Props/Emits 类型完整
2. Store 类型定义清晰
3. API 响应类型统一
