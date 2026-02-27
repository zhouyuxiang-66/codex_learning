# Codex CLI 使用教程（按你的启动方式）

本教程按你指定的方式来写：
先进入待操作目录，在该目录打开 `cmd`，然后输入 `codex` 开始使用。

## 1. 你要的标准启动流程

1. 进入项目目录（例如 `D:\my_project`）
2. 在这个目录里打开 `cmd`
3. 输入：
```bash
codex
```
4. 回车后进入 Codex CLI 交互界面，直接输入需求即可

示例（在 `cmd` 中）：
```bash
cd /d D:\my_project
codex
```

## 2. 常用启动方式

- 当前目录直接启动：
```bash
codex
```

- 启动时直接给任务：
```bash
codex "帮我先阅读项目结构，再列出重构计划"
```

- 指定工作目录启动（不先 `cd` 也可以）：
```bash
codex -C D:\my_project
```

- 查看帮助与版本：
```bash
codex --help
codex -V
```

## 3. 可用功能总览（主命令）

以下是 Codex CLI 的主要功能：

- `exec`：非交互执行任务
- `review`：非交互代码审查
- `login` / `logout`：登录与退出登录
- `mcp`：管理外部 MCP 服务
- `mcp-server`：把 Codex 作为 MCP Server 启动
- `app-server`：实验性应用服务工具
- `completion`：生成 Shell 自动补全脚本
- `sandbox`：在沙箱中运行命令
- `debug`：调试相关工具
- `apply`：应用任务产生的 diff 到本地仓库
- `resume`：恢复历史会话
- `fork`：从历史会话分叉新会话
- `cloud`：实验性的 Codex Cloud 任务能力
- `features`：查看和切换 feature flags

## 4. 每个功能怎么用（操作教程）

### 4.1 交互模式（最常用）

```bash
codex
```

进入后可以直接说：
- “帮我修复这个报错”
- “给我补齐这个模块的单元测试”
- “先分析代码结构，再给优化方案”

### 4.2 非交互执行：`exec`

```bash
codex exec "修复 tests 目录下失败用例"
```

从标准输入读取任务：
```bash
echo 请检查安全风险并输出修复建议 | codex exec -
```

输出 JSON 事件流：
```bash
codex exec "总结本次改动" --json
```

把最后一条消息写入文件：
```bash
codex exec "输出最终结论" -o last_message.txt
```

### 4.3 代码审查：`review`

审查未提交改动：
```bash
codex review --uncommitted
```

基于分支审查：
```bash
codex review --base main
```

审查指定提交：
```bash
codex review --commit <SHA>
```

### 4.4 登录与认证：`login` / `logout`

```bash
codex login
codex login status
codex logout
```

用 API Key（stdin）登录：
```bash
echo %OPENAI_API_KEY% | codex login --with-api-key
```

### 4.5 会话管理：`resume` / `fork`

恢复最近会话：
```bash
codex resume --last
```

恢复指定会话：
```bash
codex resume <SESSION_ID>
```

基于最近会话分叉：
```bash
codex fork --last
```

### 4.6 MCP：`mcp` / `mcp-server`

列出 MCP 服务：
```bash
codex mcp list
codex mcp list --json
```

添加 stdio MCP：
```bash
codex mcp add myserver -- my-mcp-server --port 8080
```

添加 HTTP MCP：
```bash
codex mcp add myhttp --url https://example.com/mcp
```

查看与删除：
```bash
codex mcp get myserver
codex mcp remove myserver
```

启动 Codex MCP Server：
```bash
codex mcp-server
```

### 4.7 Cloud（实验性）：`cloud`

```bash
codex cloud exec --env <ENV_ID> "重构用户服务并补全测试"
codex cloud status <TASK_ID>
codex cloud diff <TASK_ID>
codex cloud apply <TASK_ID>
codex cloud list
```

### 4.8 Features：`features`

```bash
codex features list
codex features enable <FEATURE_NAME>
codex features disable <FEATURE_NAME>
```

### 4.9 自动补全：`completion`

```bash
codex completion powershell
codex completion bash
codex completion zsh
codex completion fish
```

### 4.10 沙箱与调试：`sandbox` / `debug`

```bash
codex sandbox --help
codex sandbox windows --help
codex debug --help
codex debug app-server --help
```

### 4.11 应用补丁：`apply`

```bash
codex apply <TASK_ID>
```

## 5. 高频命令速查

```bash
codex
codex --help
codex -V
codex "帮我修这个 bug"
codex exec "补齐测试并运行"
codex review --uncommitted
codex login status
codex resume --last
codex fork --last
codex mcp list --json
codex features list
```

## 6. 推荐实战流程（按你的习惯）

1. 进入项目目录并打开 `cmd`
2. 执行 `codex`
3. 先让它分析项目，再给计划
4. 让它逐步改代码、解释改动
5. 用 `codex review --uncommitted` 做质量检查

## 7. 常见问题

- 如果在 PowerShell 里遇到执行策略限制（`codex.ps1` 被拦截），可改用：
```bash
codex.cmd
```

- 想更自动化可用：
```bash
codex --full-auto
```

- 想更保守可用：
```bash
codex -s read-only -a untrusted
```
