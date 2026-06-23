# CC Switch 初始化说明

## 工具说明

CC Switch 是 AI 编程工具配置管理器，支持 Claude Code、Codex、Gemini CLI、OpenCode 等工具的 provider 切换。

## 配置状态

- 无自定义配置文件需要保存。
- 配置通过 CC Switch 应用界面管理，导出为各工具的配置文件。

## 初始化步骤

1. 从 [CC Switch 官网](https://ccswitch.io/) 下载最新版安装包。
2. 安装到 `D:\CC Switch`。
3. 启动应用，添加 AI provider，配置 API Key 和 Base URL。
4. 导出配置到目标工具（如 opencode）。

## 注意

- 该工具管理其他工具的配置，本身不需要 init 模板。
- 导出配置后，目标工具（如 opencode）需重启生效。
