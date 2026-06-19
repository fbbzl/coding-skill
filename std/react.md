# React 标准

## 项目结构

0. 按功能域组织：`features/`、`components/`、`hooks/`、`services/`、`stores/`、`utils/`
1. 公共 UI 组件放在 `components/ui/`
2. 页面组件放在 `pages/` 或 `app/`
3. 路由使用 React Router 或 Next.js App Router

## 组件

0. 函数组件优先，类组件仅在必要时使用
1. 组件名 PascalCase，单文件组件
2. Props 使用 TypeScript 接口定义
3. 默认 props 通过解构赋值
4. 列表渲染使用稳定 key

## Hooks

0. 自定义 Hook 以 use 开头
1. 一个 Hook 职责单一
2. 依赖数组必须完整
3. useEffect 返回清理函数（需要时）
4. 避免在循环/条件中调用 Hook

## 状态管理

0. 本地状态优先 useState/useReducer
1. 跨组件状态使用 Context 或 Zustand/Redux
2. 服务端状态使用 TanStack Query / SWR
3. 状态不可变更新

## 性能

0. 使用 React.memo 避免不必要重渲染（有证据时）
1. useMemo/useCallback 按需使用，不滥用
2. 大列表使用虚拟滚动
3. 图片懒加载
4. 代码分割和路由懒加载

## 样式

0. 使用 CSS Modules / Styled-components / Tailwind
1. 避免全局样式冲突
2. 主题变量统一管理
3. 响应式使用 Tailwind 断点或 CSS media query

## 表单

0. 使用 React Hook Form 或 Formik
1. 校验使用 Zod / Yup
2. 提交状态loading、error、success处理完整
3. 受控组件与非受控组件选择合理

## 测试

0. 使用 Vitest + React Testing Library
1. 测试组件行为，而不是实现细节
2. 用户事件使用 @testing-library/user-event
3. Mock API 调用

## 错误边界

0. 顶层配置 Error Boundary
1. 局部 Error Boundary 隔离关键模块
2. 错误信息友好，引导用户恢复

## 类型

0. TypeScript strict 模式
1. Props 类型完整
2. API 响应类型统一定义
3. 避免 any，用 unknown 兜底
