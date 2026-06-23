---
name: mcp-server-dev
version: 1.0.0
type: technical-skill
scope: ai-coding
description: "为内部系统、数据库、工具链编写 MCP Server 的标准流程"
tags: [mcp, server, ai-coding, opencode, integration]
author: coding-skill
---

# MCP Server 开发规范

## 定位

MCP（Model Context Protocol）Server 是 AI 编码助手与内部系统、数据库、工具链之间的标准接口。本 skill 规范如何为项目设计、实现、测试和发布 MCP Server。

## 适用场景

- 让 opencode 查询内部知识库、文档或代码库
- 让 opencode 调用内部 API、CLI 或脚本
- 让 opencode 读取数据库元数据或运行只读查询
- 让 opencode 操作构建、部署、测试等工程化工具

## MCP 核心概念

| 概念 | 用途 |
|---|---|
| `tools` | AI 可调用的函数或操作，带输入输出 schema |
| `resources` | AI 可读取的数据源，如文件、文档、配置 |
| `prompts` | 预定义提示词模板，供 AI 或用户引用 |
| `sampling` | Server 请求 AI 生成内容的能力 |

优先使用 `tools` 和 `resources`，谨慎使用 `sampling`。

## 技术栈选择

| 场景 | 推荐 |
|---|---|
| 内部 TypeScript/Node 项目 | `@modelcontextprotocol/sdk` + TypeScript |
| 内部 Python 项目 | `mcp` Python SDK |
| 内部 Java 项目 | 官方 Java SDK 或 Spring Boot 封装 |
| 快速原型 | TypeScript SDK + stdio transport |
| 服务化部署 | TypeScript/Java SDK + SSE transport |

## 项目结构模板

```
mcp-servers/<server-name>/
├── src/
│   ├── index.ts              # 入口，transport 注册
│   ├── server.ts             # MCP Server 实例
│   ├── tools/                # tools 定义与实现
│   ├── resources/            # resources 定义与实现
│   ├── prompts/              # prompts 定义
│   ├── clients/              # 内部系统客户端封装
│   └── types.ts              # 共享类型
├── tests/
│   ├── tools.test.ts
│   └── resources.test.ts
├── README.md                 # 使用说明、配置方式、安全声明
├── package.json
└── tsconfig.json
```

## 设计原则

- 一个 tool/resource 只做一件事
- 输入输出必须定义 JSON Schema，禁止模糊字段
- 默认只读，写操作必须显式声明并加权限校验
- 所有对外接口必须有超时和熔断设计
- 错误信息必须结构化，不暴露内部堆栈

## 安全规范

- MCP Server 不存储 secrets，运行时从环境变量或安全密钥管理读取
- 写操作 tool 必须二次确认，禁止自动执行危险操作
- 数据库 tool 默认只读，写操作需要单独启用开关
- 所有请求必须记录审计日志：调用方、参数摘要、结果摘要、时间
- 禁止返回完整 secrets、密码、私钥、token

## 认证与权限

- stdio transport：依赖宿主进程（如 opencode）的权限，Server 本身不认证
- SSE transport：必须配置 API Key / OAuth / mTLS 至少一种
- 每个 tool 声明所需权限级别，调用前校验
- 对敏感系统，采用最小权限原则分配服务账号

## 错误处理与日志

- 错误码统一：`INVALID_INPUT`、`NOT_FOUND`、`INTERNAL_ERROR`、`PERMISSION_DENIED`、`TIMEOUT`
- 日志级别：`debug` 记录参数摘要，`info` 记录调用成功，`warn` 记录可恢复失败，`error` 记录异常
- 生产环境关闭 `debug` 日志
- 错误响应不返回内部堆栈或数据库详情

## 测试要求

- 每个 tool 必须有单元测试，覆盖正常路径、异常路径、边界输入
- 必须有集成测试验证与内部系统的真实连接
- 必须测试权限不足场景
- 必须测试超时和重试行为

## 发布与集成

- 发布前更新 README：功能清单、配置示例、权限说明、运行命令
- 在 `opencode.jsonc` 中注册 MCP Server：

```json
{
  "mcpServers": {
    "my-server": {
      "command": "node",
      "args": ["path/to/mcp-servers/my-server/dist/index.js"]
    }
  }
}
```

- 与 `skills/dev/SKILL.md` 联动：dev 子代理负责实现 MCP Server，按本规范输出设计文档和代码

## 质量门禁

- [ ] 所有 tool/resource 都有 JSON Schema
- [ ] 所有写操作都有权限校验和二次确认
- [ ] 单元测试覆盖率不低于 80%
- [ ] 集成测试通过
- [ ] README 完整
- [ ] 已在 opencode.jsonc 注册并验证可用

## 反模式

- 不要在一个 tool 里做多个不相关的事情
- 不要让 MCP Server 直接暴露原始 SQL 给 AI 自由执行
- 不要把 secrets 硬编码在代码或配置中
- 不要在生产环境开启 `sampling` 而不加审批
