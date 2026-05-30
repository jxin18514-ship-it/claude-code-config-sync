# Claude Code VS Code 权限配置同步包

> 用于在新电脑上快速复现 Claude Code 的完整权限配置，无需逐项手动授权。

## 📦 包含文件

| 文件 | 路径 | 用途 |
|------|------|------|
| `settings.json` | `%USERPROFILE%\.claude\settings.json` | Claude Code 全局权限 |
| `settings.local.json` | `%USERPROFILE%\.claude\settings.local.json` | 本地覆盖权限（视频处理专用） |
| `vscode-settings.json` | VS Code `settings.json` 中追加 | VS Code 中 Claude 面板位置 |
| `claude_desktop_config.json` | `%LOCALAPPDATA%\Claude-3p\claude_desktop_config.json` | Claude 扩展桌面配置 |

## 🚀 安装步骤

### 第一步：替换占位符

**必须改！** 在复制文件之前，把下面占位符换成你自己的实际值：

| 占位符 | 说明 | 在哪里 |
|--------|------|--------|
| `YOUR_API_KEY_HERE` | 你的 API Key | `settings.json` |
| `YOUR_API_BASE_URL` | API 代理地址（如 `http://127.0.0.1:8080/anthropic`） | `settings.json` |
| `YOUR_MODEL_NAME` | 模型名称（如 `deepseek-v4-pro`） | `settings.json` |
| `YOUR_WORK_DIRS` | 你的工作目录路径 | `settings.json` |

> ⚠️ **不要直接用我的路径！** 把 `D:/5.5视频处理/...`、`E:/video_work/...` 等换成你自己的。

### 第二步：复制文件

```powershell
# 1. Claude Code 全局设置
copy settings.json $env:USERPROFILE\.claude\settings.json

# 2. （可选）本地覆盖设置 — 如果你不需要视频处理工作流，跳过这个
copy settings.local.json $env:USERPROFILE\.claude\settings.local.json

# 3. Claude 桌面配置
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

全局 `settings.json` 设置的权限模式为 **`bypassPermissions`**，意味着：
- 所有 Bash 命令直接执行，不弹确认框
- 所有文件读写操作免确认
- WebSearch / WebFetch 免确认
- 子代理（Agent）免确认
- MCP 浏览器工具免确认

> 如果你希望保留部分确认弹窗，把 `defaultMode` 改为 `"acceptEdits"` 或 `"default"`。

## 📋 设置清单速查

| 设置项 | 当前模板值 |
|--------|-----------|
| defaultMode | `bypassPermissions` |
| effortLevel | `xhigh` |
| 允许的工具 | `*`, `Bash(*)`, `Read/Write/Edit`, `WebSearch/WebFetch(*)`, `Agent(*)`, `Skill(*)`, `mcp__playwright__*` |
| WebSearch | 启用 |
| 定时任务 | 启用 |
| 侧边栏模式 | `task` |
