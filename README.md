# Claude Code 配置同步模板

> 低成本、安全、分层模型的 Claude Code 配置模板，用于在新电脑上快速同步配置。

## 🧠 默认分层模型策略

**核心原则：主任务用 Pro，轻量任务用 Flash，不要全部用 Pro 烧钱。**

| 角色 | 模型 | 说明 |
|------|------|------|
| 主模型 (ANTHROPIC_MODEL) | `deepseek-v4-pro` | 当前对话/主推理 |
| Opus (ANTHROPIC_DEFAULT_OPUS_MODEL) | `deepseek-v4-pro` | 复杂任务时切 Opus |
| Sonnet (ANTHROPIC_DEFAULT_SONNET_MODEL) | `deepseek-v4-flash` | 常规编码任务 |
| Haiku (ANTHROPIC_DEFAULT_HAIKU_MODEL) | `deepseek-v4-flash` | 轻量快速响应 |
| Subagent (CLAUDE_CODE_SUBAGENT_MODEL) | `deepseek-v4-flash` | 子代理并行任务 |
| effortLevel | `medium` | 日常够用，不烧钱 |

### ⚠️ 成本警告

- **不要把所有模型都改成 `deepseek-v4-pro`**：Sonnet/Haiku/Subagent 走 Flash 即可，全部 Pro 会大幅增加 API 费用
- **不要长期使用 `xhigh`**：日常 `medium` 足够，只有重要复杂任务才临时改
- **不要把真实 API Key 提交到 GitHub**：提交前确认密钥已替换为占位符

## 📦 包含文件

| 文件 | 部署路径 | 用途 |
|------|----------|------|
| `settings.json` | `%USERPROFILE%\.claude\settings.json` | Claude Code 全局配置 |
| `vscode-settings.json` | VS Code `settings.json` 中追加 | VS Code 中 Claude 面板位置 |
| `claude_desktop_config.json` | `%LOCALAPPDATA%\Claude-3p\claude_desktop_config.json` | Claude 扩展桌面配置 |

## 🚀 安装步骤

### 第一步：替换占位符

**必须改！** 在复制文件之前，把下面占位符换成你自己的实际值：

| 占位符 | 说明 | 位置 |
|--------|------|------|
| `YOUR_DEEPSEEK_API_KEY_HERE` | 你的 DeepSeek API Key | `settings.json` |
| `YOUR_WORK_DIR_1` / `YOUR_WORK_DIR_2` | 你的工作目录路径 | `settings.json` |

> ⚠️ 如果使用本地代理，`ANTHROPIC_BASE_URL` 保持 `http://127.0.0.1:8080/anthropic` 不变即可。

### 第二步：复制文件

```powershell
# 1. Claude Code 全局设置
copy settings.json $env:USERPROFILE\.claude\settings.json

# 2. Claude 桌面配置
copy claude_desktop_config.json $env:LOCALAPPDATA\Claude-3p\claude_desktop_config.json
```

### 第三步：VS Code 设置

在 VS Code 的 `settings.json` 中添加：

```json
"claudeCode.preferredLocation": "panel"
```

### 第四步：安装 VS Code 扩展

在 VS Code 扩展市场搜索并安装 **Claude Code**（作者：Anthropic）。

### 第五步：重启 VS Code

完全关闭 VS Code，重新打开，检查 Claude 面板是否正常加载。

---

## 🔐 权限说明

全局 `settings.json` 默认权限模式为 **`acceptEdits`**，这是一个平衡的选择：

- 文件编辑 (`Edit`) 需要确认
- Bash 命令需要确认
- 子代理 (Agent) 需要确认
- 基础读取 (Read/Glob/Grep) 免确认

> ⚠️ **Bash / Agent / 自动编辑有风险**：如果你改为 `bypassPermissions`，Claude 可以在不弹窗的情况下执行任意命令、修改任意文件。在大项目运行前建议保留人工确认。
>
> 如需完全免确认，把 `defaultMode` 改为 `"bypassPermissions"`，但请自行承担风险。

## 🌐 本地代理说明

本配置默认使用本地代理 `http://127.0.0.1:8080/anthropic`：

```
Claude Code → 127.0.0.1:8080 → api.deepseek.com/anthropic
```

| 项目 | 说明 |
|------|------|
| 代理路径 | `C:\deepseek-proxy\proxy.py`（FastAPI + httpx） |
| 功能 | Anthropic system message 格式兼容 + 透明转发 |
| 模型字段 | 代理**不修改** model 字段，仅透传 |
| 启动方式 | `python proxy.py` 或开机自启 |

如需直连 DeepSeek，手动将 `ANTHROPIC_BASE_URL` 改为：

```
https://api.deepseek.com/anthropic
```

但推荐先保留本地代理，以确保 Claude Code 的 system message 兼容性。

---

## 📋 设置清单速查

| 设置项 | 当前模板值 |
|--------|-----------|
| defaultMode | `acceptEdits` |
| effortLevel | `medium` |
| 主模型 | `deepseek-v4-pro` |
| Sonnet/Haiku/Subagent | `deepseek-v4-flash` |
| 允许的工具 | Bash, Read, Write, Edit, Glob, Grep, WebSearch, WebFetch, Skill, Agent, MCP Playwright |
| 侧边栏模式 | `task` |
