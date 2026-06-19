# Git 使用标准

## 分支模型

0. 主分支：`main` 或 `master`，始终可部署
1. 开发分支：`develop`（可选）
2. 功能分支：`feature/简短描述`
3. 修复分支：`bugfix/简短描述` 或 `fix/简短描述`
4. 热修复分支：`hotfix/简短描述`
5. 发布分支：`release/版本号`

## 提交规范

0. 使用 Conventional Commits 格式：
   ```
   type(scope): subject

   body

   footer
   ```
1. type 类型：feat、fix、docs、style、refactor、test、chore、perf、ci
2. subject 使用祈使句，首字母小写，不超过 50 字符
3. body 说明做了什么和为什么
4. footer 关联 issue：`Closes #123`
5. 单次提交只做一件事

## 提交示例

```
feat(order): add cancel order API

- add PUT /orders/{id}/cancel endpoint
- validate order status before cancellation
- send notification after cancellation

Closes #456
```

## PR 规范

0. PR 标题清晰说明变更内容
1. PR 描述包含变更背景、范围、验证方式
2. 必须关联相关 issue 或需求文档
3. 每个 PR 不宜过大，建议不超过 400 行变更
4. 必须经过至少一人代码审查
5. CI 通过后方可合并

## 代码合并

0. 优先使用 Squash Merge 保持主分支提交历史整洁
2. 合并前 rebase 到目标分支最新提交
3. 解决冲突后必须再次验证
4. 禁止合并未通过测试的代码
5. 禁止在功能分支上直接 force push

## 版本标签

0. 使用语义化版本：`v1.2.3`
1. 发布时打标签并写 Release Note
2. 标签与 CI/CD 版本对应

## 代码回滚

0. 线上问题优先回滚再修复
1. 回滚使用 revert 生成新提交，不 force push
2. 记录回滚原因和影响范围
