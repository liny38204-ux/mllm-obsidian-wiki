---
title: "luongnv89/claude-howto: A visual, example-driven guide to Claude Code — from basic concepts to advanced agents, with copy-paste templates that bring immediate value."
source: "https://github.com/luongnv89/claude-howto/tree/main/06-hooks"
author:
  - "[[luongnv89]]"
published:
created: 2026-05-04
description: "A visual, example-driven guide to Claude Code — from basic concepts to advanced agents, with copy-paste templates that bring immediate value. - luongnv89/claude-howto"
tags:
  - "clippings"
---
![Claude How To](https://github.com/luongnv89/claude-howto/raw/main/resources/logos/claude-howto-logo.svg)

## 钩子

钩子是自动化脚本，会在 Claude Code 会话中响应特定事件执行。它们支持自动化、验证、权限管理和自定义工作流程。

## 概述

钩子是自动化操作（shell命令、HTTP webhook、LLM提示、MCP工具调用或子代理评估），当Claude代码中发生特定事件时自动执行。他们接收JSON输入，并通过退出代码和JSON输出传达结果。

**主要特点：**

- 事件驱动自动化
- 基于 JSON 的输入/输出
- 支持、、、和挂钩类型的支持。 `command` `http` `mcp_tool` `prompt` `agent`
- 工具专用钩子的模式匹配

## 配置

钩子在设置文件中配置，具有特定结构：

- `~/.claude/settings.json` \- 用户设置（所有项目）
- `.claude/settings.json` \- 项目设置（可共享，已提交）
- `.claude/settings.local.json` \- 本地项目设置（未提交）
- 托管策略——全组织设置
- 插件 - 插件作用域钩子 `hooks/hooks.json`
- Skill/Agent 前言 - 组件生命周期钩子

### 基本配置结构

```
{
  "hooks": {
    "EventName": [
      {
        "matcher": "ToolPattern",
        "hooks": [
          {
            "type": "command",
            "command": "your-command-here",
            "timeout": 60
          }
        ]
      }
    ]
  }
}
```

**主要领域：**

| 场地 | 描述 | 示例 |
| --- | --- | --- |
| `matcher` | 与工具名称匹配的图案（大小写相关） | `"Write"`,,`"Edit\|Write"` `"*"` |
| `hooks` | 钩子定义的数组 | `[{ "type": "command", ... }]` |
| `type` | 钩子类型：（bash）、（LLM）、（webhook）、（MCP 工具调用，v2.1.118+）、或（子代理） `"command"` `"prompt"` `"http"` `"mcp_tool"` `"agent"` | `"command"` |
| `command` | 执行 Shell 命令 | `"$CLAUDE_PROJECT_DIR/.claude/hooks/format.sh"` |
| `timeout` | 可选超时秒（默认60秒） | `30` |
| `once` | 如果 ，则每次只运行一次钩子 `true` | `true` |

### 匹配模式

| 模式 | 描述 | 示例 |
| --- | --- | --- |
| 精确字符串 | 匹配特定工具 | `"Write"` |
| 正则表达式模式 | 匹配多种工具 | `"Edit\|Write"` |
| 外卡 | 匹配所有工具 | `"*"` 或 `""` |
| MCP 工具 | 服务器和工具模式 | `"mcp__memory__.*"` |

**指令加载匹配器值：**

| 匹配值 | 描述 |
| --- | --- |
| `session_start` | 会话启动时加载的指令 |
| `nested_traversal` | 嵌套目录遍历期间加载的指令 |
| `path_glob_match` | 通过路径团图模式匹配加载的指令 |

## 钩子类型

Claude 代码支持五种钩子类型：

### 指令钩

默认的挂钩类型。执行shell命令，并通过JSON、stdin/stdout和退出代码进行通信。

```
{
  "type": "command",
  "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/validate.py\"",
  "timeout": 60
}
```

### HTTP 钩子

> 在 v2.1.63 中添加。

远程webhook端点接收与命令钩子相同的JSON输入。HTTP 钩子 POST JSON 到 URL，并接收 JSON 响应。当启用沙箱时，HTTP 钩子会通过沙箱路由。在URL中进行环境变量插值需要显式列表以确保安全。 `allowedEnvVars`

```
{
  "hooks": {
    "PostToolUse": [{
      "type": "http",
      "url": "https://my-webhook.example.com/hook",
      "matcher": "Write"
    }]
  }
}
```

**主要特性：**

- `"type": "http"` —— 将此标识为 HTTP 钩子
- `"url"` —— webhook 端点 URL
- 启用沙箱时，路由通过沙盒
- 要求对URL中任何环境变量插值都必须有明确列表 `allowedEnvVars`

### 提示钩子

LLM评估提示，其中钩子内容是Claude评估的提示。主要用于智能任务完成检查和事件。 `Stop` `SubagentStop`

```
{
  "type": "prompt",
  "prompt": "Evaluate if Claude completed all requested tasks.",
  "timeout": 30
}
```

LLM评估提示并返回结构化决策（详见 [基于提示的钩子](#prompt-based-hooks) ）。

### MCP工具钩

> 在 v2.1.118 中添加。

该类型直接调用已配置的MCP工具;配置引用的是MCP服务器和工具名称，而非shell命令或URL。当验证或反应逻辑已经存在于你配置的MCP服务器中时，这非常有用。 `mcp_tool`

```
{
  "matcher": "Edit",
  "hooks": [{
    "type": "mcp_tool",
    "server": "my-mcp-server",
    "tool": "validate_edit"
  }]
}
```

**主要特性：**

- `"type": "mcp_tool"` —— 识别此为 MCP 工具钩子
- `"server"` —— 配置好的MCP服务器名称
- `"tool"` —— 该服务器上用于调用的工具名称

钩子输入（工具名称、工具输入、会话上下文）作为MCP工具的参数传递。请参见 [MCP 服务器设置以配置 MCP](https://github.com/luongnv89/claude-howto/blob/main/05-mcp/README.md) 服务器。

### 特工胡克斯

基于子代理的验证钩子，生成专门的代理来评估状态或执行复杂检查。与提示钩子（单回合LLM评估）不同，代理钩子可以使用工具并执行多步推理。

```
{
  "type": "agent",
  "prompt": "Verify the code changes follow our architecture guidelines. Check the relevant design docs and compare.",
  "timeout": 120
}
```

**主要特性：**

- `"type": "agent"` —— 识别出这是代理钩子
- `"prompt"` ——子代理的任务描述
- 代理可以使用工具（Read、Grep、Bash 等）来进行评估
- 返回类似提示钩子的结构化决策

## 钩子事件

Claude Code 支持 **28 个钩子事件** ：

| 事件 | 触发时 | 匹配器输入 | 罐头阻挡 | 通用用途 |
| --- | --- | --- | --- | --- |
| **SessionStart** | 会话开始/恢复/清理/压缩 | 创业/简历/清晰/简易 | 不 | 环境设置 |
| **指令已加载** | CLAUDE.md 或规则文件加载后 | （无） | 不 | 修改/过滤指令 |
| **用户提示提交** | 用户提交提示 | （无） | 是的 | 验证提示 |
| **用户提示扩展** | 用户提示词扩展（例如，提及、斜杠命令已解决） `@` | （无） | 是的 | 转换或检查扩展提示 |
| **工具使用前** | 工具执行前 | 工具名称 | 是的（允许/拒绝/请求） | 验证，修改输入 |
| **许可请求** | 权限对话框显示 | 工具名称 | 是的 | 自动批准/拒绝 |
| **许可被拒** | 用户拒绝权限提示 | 工具名称 | 不 | 日志、分析、政策执行 |
| **工具使用后** | 工具成功后 | 工具名称 | 不 | 补充背景和反馈 |
| **工具使用失败后** | 工具执行失败 | 工具名称 | 不 | 错误处理与日志记录 |
| **工具批处理后** | 一批工具使用完成后 | （无） | 不 | 汇总报告，批量验证 |
| **通知** | 已发送通知 | 通知类型 | 不 | 自定义通知 |
| **SubagentStart** | Subagent生成 | 代理类型名称 | 不 | 子代理设置 |
| **SubagentStop（子代理停止）** | 分代理结束 | 代理类型名称 | 是的 | 子代理验证 |
| **停下** | 克洛德回应完毕 | （无） | 是的 | 任务完成检查 |
| **停止失败** | API 错误结束 Turn。 | （无） | 不 | 错误恢复与日志记录 |
| **队友Idle** | 特工队友闲置 | （无） | 是的 | 队友协调 |
| **任务完成** | 任务标记完成 | （无） | 是的 | 任务后动作 |
| **任务创建** | 任务通过TaskCreate创建 | （无） | 不 | 任务跟踪与日志 |
| **ConfigChange** | 配置文件变更 | （无） | 是的（政策除外） | React to config 更新 |
| **CwdChanged（变革之声）** | 工作目录变更 | （无） | 不 | 目录特定设置 |
| **文件已更改** | 观看文件变更 | （无） | 不 | 文件监控，重建 |
| **预紧凑型** | 上下文压缩之前 | 手动/自动挡 | 不 | 预紧作用量 |
| **后紧凑** | 压缩完毕后 | （无） | 不 | 紧致后作用 |
| **WorktreeCreate** | 工作树的创建 | （无） | 是的（路径返回） | 工作树初始化 |
| **工作树移除** | 工作树被移除 | （无） | 不 | 工作树清理 |
| **引发** | MCP服务器请求用户输入 | （无） | 是的 | 输入验证 |
| **引发结果** | 用户对引发的响应 | （无） | 是的 | 响应处理 |
| **会话结束** | 会话终止 | （无） | 不 | 清理，最终伐木 |

> **PostToolUse 时长（v2.1.119）：** 以及钩子输入现在包括——详情请参见 [PostToolUse](#posttooluse) 部分。 `PostToolUse` `PostToolUseFailure` `duration_ms`

### 工具使用前

运行于Claude创建工具参数后、处理前。用它来验证或修改工具输入。

**配置：**

```
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/validate-bash.py"
          }
        ]
      }
    ]
  }
}
```

**常见匹配器：** ， ， `Task` `Bash` `Glob` `Grep` `Read` `Edit` `Write` `WebFetch` `WebSearch`

**输出控制：**

- `permissionDecision` ： ， ， 或 `"allow"` `"deny"` `"ask"`
- `permissionDecisionReason` ：判决说明
- `updatedInput` ：修改过的工具输入参数

### 工具使用后

工具完成后立即运行。用于验证、日志记录或向Claude提供上下文。

**配置：**

```
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/security-scan.py"
          }
        ]
      }
    ]
  }
}
```

**输出控制：**

- `"block"` 决策提示克洛德反馈
- `additionalContext` ：为克劳德补充了背景

**附加输入字段（v2.1.119）：**

| 场地 | 类型 | 描述 |
| --- | --- | --- |
| `duration_ms` | 人数 | 工具执行时间以毫秒计。不包括在权限提示和 PreToolUse 钩子执行上所花费的时间。两者都有，钩子也一样。 `PostToolUse` `PostToolUseFailure` |

### 用户提示提交

当用户提交提示时运行，Claude 尚未处理。

**配置：**

```
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/validate-prompt.py"
          }
        ]
      }
    ]
  }
}
```

**输出控制：**

- `decision` ： 以阻止处理 `"block"`
- `reason` ：如果被屏蔽，请解释
- `additionalContext` ：提示词中添加了上下文

### 停止和Subagent停止

当 Claude 完成响应（Stop）或子代理完成（SubagentStop）时运行。支持基于提示的评估，用于智能任务完成检查。

**附加输入字段：** 和 钩子都会在其 JSON 输入中获得一个字段，包含 Claude 或子代理在停止前发出的最后一条消息。这对于评估任务完成度非常有用。 `Stop` `SubagentStop` `last_assistant_message`

**配置：**

```
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "prompt",
            "prompt": "Evaluate if Claude completed all requested tasks.",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```

### SubagentStart

当子代理开始执行时运行。匹配输入是代理类型名称，允许钩子针对特定子代理类型。

**配置：**

```
{
  "hooks": {
    "SubagentStart": [
      {
        "matcher": "code-review",
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/subagent-init.sh"
          }
        ]
      }
    ]
  }
}
```

### SessionStart

会话开始或恢复时运行。可以持久化环境变量。

**匹配者：** ， ， ， `startup` `resume` `clear` `compact`

**特别内容：** 用于持久化环境变量（也可在和钩子中获得）： `CLAUDE_ENV_FILE` `CwdChanged` `FileChanged`

```
#!/bin/bash
if [ -n "$CLAUDE_ENV_FILE" ]; then
  echo 'export NODE_ENV=development' >> "$CLAUDE_ENV_FILE"
fi
exit 0
```

### 会话结束

会话结束时运行以执行清理或最终日志。无法阻止终止。

**理由字段值：**

- `clear` \- 用户清除会话
- `logout` \- 用户登出
- `prompt_input_exit` \- 用户通过提示输入退出
- `other` \- 其他原因

**配置：**

```
{
  "hooks": {
    "SessionEnd": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR/.claude/hooks/session-cleanup.sh\""
          }
        ]
      }
    ]
  }
}
```

### 通知事件

通知事件的匹配器更新：

- `permission_prompt` \- 权限请求通知
- `idle_prompt` \- 空闲状态通知
- `auth_success` \- 认证成功
- `elicitation_dialog` \- 向用户显示的对话

## 组件式钩子

钩子可以附加到前言中的特定组件（技能、特工、命令）：

**在 SKILL.md、agent.md 或 command.md 中：**

```
---
name: secure-operations
description: Perform operations with security checks
hooks:
  PreToolUse:
    - matcher: "Bash"
      hooks:
        - type: command
          command: "./scripts/check.sh"
          once: true  # Only run once per session
---
```

**组件钩子支持的事件：** ， ， `PreToolUse` `PostToolUse` `Stop`

这使得可以直接在使用它们的组件中定义钩子，保持相关代码的一致性。

### 子代理前件中的钩子

当子代理的前置中定义了钩子时，它会自动转换为该子代理的范围钩子。这确保了停止钩子只在该子代理完成时触发，而非主会话停止时触发。 `Stop` `SubagentStop`

```
---
name: code-review-agent
description: Automated code review subagent
hooks:
  Stop:
    - hooks:
        - type: prompt
          prompt: "Verify the code review is thorough and complete."
  # The above Stop hook auto-converts to SubagentStop for this subagent
---
```

## 许可请求事件

处理自定义输出格式的权限请求：

```
{
  "hookSpecificOutput": {
    "hookEventName": "PermissionRequest",
    "decision": {
      "behavior": "allow|deny",
      "updatedInput": {},
      "message": "Custom message",
      "interrupt": false
    }
  }
}
```

## 钩子输入与输出

### JSON 输入（通过 stdin）

所有钩子都通过stdin接收JSON输入：

```
{
  "session_id": "abc123",
  "transcript_path": "/path/to/transcript.jsonl",
  "cwd": "/current/working/directory",
  "permission_mode": "default",
  "hook_event_name": "PreToolUse",
  "tool_name": "Write",
  "tool_input": {
    "file_path": "/path/to/file.js",
    "content": "..."
  },
  "tool_use_id": "toolu_01ABC123...",
  "agent_id": "agent-abc123",
  "agent_type": "main",
  "worktree": "/path/to/worktree"
}
```

**Common fields:**

| Field | Description |
| --- | --- |
| `session_id` | Unique session identifier |
| `transcript_path` | Path to the conversation transcript file |
| `cwd` | Current working directory |
| `hook_event_name` | Name of the event that triggered the hook |
| `agent_id` | Identifier of the agent running this hook |
| `agent_type` | Type of agent (, subagent type name, etc.) `"main"` |
| `worktree` | Path to the git worktree, if the agent is running in one |

### Exit Codes

| Exit Code | Meaning | Behavior |
| --- | --- | --- |
| **0** | Success | Continue, parse JSON stdout |
| **2** | Blocking error | Block operation, stderr shown as error |
| **Other** | Non-blocking error | Continue, stderr shown in verbose mode |

### JSON Output (stdout, exit code 0)

```
{
  "continue": true,
  "stopReason": "Optional message if stopping",
  "suppressOutput": false,
  "systemMessage": "Optional warning message",
  "hookSpecificOutput": {
    "hookEventName": "PreToolUse",
    "permissionDecision": "allow",
    "permissionDecisionReason": "File is in allowed directory",
    "updatedInput": {
      "file_path": "/modified/path.js"
    }
  }
}
```

> **Scope (v2.1.121+):** is now honored for **all** tools, not just MCP tools. A hook on,,, etc. can rewrite the tool's output before Claude sees it — useful for redacting secrets, normalizing diffs, or filtering noisy command output. Example (strip ANSI color codes from a output):`hookSpecificOutput.updatedToolOutput` `PostToolUse` `Bash` `Edit` `Read` `Bash`
> 
> ```
> {
>   "hookSpecificOutput": {
>     "hookEventName": "PostToolUse",
>     "updatedToolOutput": "<plain-text output with ANSI escapes removed>"
>   }
> }
> ```

## Environment Variables

| Variable | Availability | Description |
| --- | --- | --- |
| `CLAUDE_PROJECT_DIR` | All hooks | Absolute path to project root |
| `CLAUDE_ENV_FILE` | SessionStart, CwdChanged, FileChanged | File path for persisting env vars |
| `CLAUDE_CODE_REMOTE` | All hooks | `"true"` if running in remote environments |
| `${CLAUDE_PLUGIN_ROOT}` | Plugin hooks | Path to plugin directory |
| `${CLAUDE_PLUGIN_DATA}` | Plugin hooks | Path to plugin data directory |
| `CLAUDE_CODE_SESSIONEND_HOOKS_TIMEOUT_MS` | SessionEnd hooks | Configurable timeout in milliseconds for SessionEnd hooks (overrides default) |

## Prompt-Based Hooks

For and events, you can use LLM-based evaluation:`Stop` `SubagentStop`

```
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "prompt",
            "prompt": "Review if all tasks are complete. Return your decision.",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```

**LLM Response Schema:**

```
{
  "decision": "approve",
  "reason": "All tasks completed successfully",
  "continue": false,
  "stopReason": "Task complete"
}
```

## Examples

### Example 1: Bash Command Validator (PreToolUse)

**File:** `.claude/hooks/validate-bash.py`

```
#!/usr/bin/env python3
import json
import sys
import re

BLOCKED_PATTERNS = [
    (r"\brm\s+-rf\s+/", "Blocking dangerous rm -rf / command"),
    (r"\bsudo\s+rm", "Blocking sudo rm command"),
]

def main():
    input_data = json.load(sys.stdin)

    tool_name = input_data.get("tool_name", "")
    if tool_name != "Bash":
        sys.exit(0)

    command = input_data.get("tool_input", {}).get("command", "")

    for pattern, message in BLOCKED_PATTERNS:
        if re.search(pattern, command):
            print(message, file=sys.stderr)
            sys.exit(2)  # Exit 2 = blocking error

    sys.exit(0)

if __name__ == "__main__":
    main()
```

**Configuration:**

```
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/validate-bash.py\""
          }
        ]
      }
    ]
  }
}
```

### Example 2: Security Scanner (PostToolUse)

**File:** `.claude/hooks/security-scan.py`

```
#!/usr/bin/env python3
import json
import sys
import re

SECRET_PATTERNS = [
    (r"password\s*=\s*['\"][^'\"]+['\"]", "Potential hardcoded password"),
    (r"api[_-]?key\s*=\s*['\"][^'\"]+['\"]", "Potential hardcoded API key"),
]

def main():
    input_data = json.load(sys.stdin)

    tool_name = input_data.get("tool_name", "")
    if tool_name not in ["Write", "Edit"]:
        sys.exit(0)

    tool_input = input_data.get("tool_input", {})
    content = tool_input.get("content", "") or tool_input.get("new_string", "")
    file_path = tool_input.get("file_path", "")

    warnings = []
    for pattern, message in SECRET_PATTERNS:
        if re.search(pattern, content, re.IGNORECASE):
            warnings.append(message)

    if warnings:
        output = {
            "hookSpecificOutput": {
                "hookEventName": "PostToolUse",
                "additionalContext": f"Security warnings for {file_path}: " + "; ".join(warnings)
            }
        }
        print(json.dumps(output))

    sys.exit(0)

if __name__ == "__main__":
    main()
```

### Example 3: Auto-Format Code (PostToolUse)

**File:** `.claude/hooks/format-code.sh`

```
#!/bin/bash

# Read JSON from stdin
INPUT=$(cat)
TOOL_NAME=$(echo "$INPUT" | python3 -c "import sys, json; print(json.load(sys.stdin).get('tool_name', ''))")
FILE_PATH=$(echo "$INPUT" | python3 -c "import sys, json; print(json.load(sys.stdin).get('tool_input', {}).get('file_path', ''))")

if [ "$TOOL_NAME" != "Write" ] && [ "$TOOL_NAME" != "Edit" ]; then
    exit 0
fi

# Format based on file extension
case "$FILE_PATH" in
    *.js|*.jsx|*.ts|*.tsx|*.json)
        command -v prettier &>/dev/null && prettier --write "$FILE_PATH" 2>/dev/null
        ;;
    *.py)
        command -v black &>/dev/null && black "$FILE_PATH" 2>/dev/null
        ;;
    *.go)
        command -v gofmt &>/dev/null && gofmt -w "$FILE_PATH" 2>/dev/null
        ;;
esac

exit 0
```

### Example 4: Prompt Validator (UserPromptSubmit)

**File:** `.claude/hooks/validate-prompt.py`

```
#!/usr/bin/env python3
import json
import sys
import re

BLOCKED_PATTERNS = [
    (r"delete\s+(all\s+)?database", "Dangerous: database deletion"),
    (r"rm\s+-rf\s+/", "Dangerous: root deletion"),
]

def main():
    input_data = json.load(sys.stdin)
    prompt = input_data.get("user_prompt", "") or input_data.get("prompt", "")

    for pattern, message in BLOCKED_PATTERNS:
        if re.search(pattern, prompt, re.IGNORECASE):
            output = {
                "decision": "block",
                "reason": f"Blocked: {message}"
            }
            print(json.dumps(output))
            sys.exit(0)

    sys.exit(0)

if __name__ == "__main__":
    main()
```

### Example 5: Intelligent Stop Hook (Prompt-Based)

```
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "prompt",
            "prompt": "Review if Claude completed all requested tasks. Check: 1) Were all files created/modified? 2) Were there unresolved errors? If incomplete, explain what's missing.",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```

### Example 6: Context Usage Tracker (Hook Pairs)

Track token consumption per request using (pre-message) and (post-response) hooks together.`UserPromptSubmit` `Stop`

**File:** `.claude/hooks/context-tracker.py`

```
#!/usr/bin/env python3
"""
Context Usage Tracker - Tracks token consumption per request.

Uses UserPromptSubmit as "pre-message" hook and Stop as "post-response" hook
to calculate the delta in token usage for each request.

Token Counting Methods:
1. Character estimation (default): ~4 chars per token, no dependencies
2. tiktoken (optional): More accurate (~90-95%), requires: pip install tiktoken
"""
import json
import os
import sys
import tempfile

# Configuration
CONTEXT_LIMIT = 128000  # Claude's context window (adjust for your model)
USE_TIKTOKEN = False    # Set True if tiktoken is installed for better accuracy

def get_state_file(session_id: str) -> str:
    """Get temp file path for storing pre-message token count, isolated by session."""
    return os.path.join(tempfile.gettempdir(), f"claude-context-{session_id}.json")

def count_tokens(text: str) -> int:
    """
    Count tokens in text.

    Uses tiktoken with p50k_base encoding if available (~90-95% accuracy),
    otherwise falls back to character estimation (~80-90% accuracy).
    """
    if USE_TIKTOKEN:
        try:
            import tiktoken
            enc = tiktoken.get_encoding("p50k_base")
            return len(enc.encode(text))
        except ImportError:
            pass  # Fall back to estimation

    # Character-based estimation: ~4 characters per token for English
    return len(text) // 4

def read_transcript(transcript_path: str) -> str:
    """Read and concatenate all content from transcript file."""
    if not transcript_path or not os.path.exists(transcript_path):
        return ""

    content = []
    with open(transcript_path, "r") as f:
        for line in f:
            try:
                entry = json.loads(line.strip())
                # Extract text content from various message formats
                if "message" in entry:
                    msg = entry["message"]
                    if isinstance(msg.get("content"), str):
                        content.append(msg["content"])
                    elif isinstance(msg.get("content"), list):
                        for block in msg["content"]:
                            if isinstance(block, dict) and block.get("type") == "text":
                                content.append(block.get("text", ""))
            except json.JSONDecodeError:
                continue

    return "\n".join(content)

def handle_user_prompt_submit(data: dict) -> None:
    """Pre-message hook: Save current token count before request."""
    session_id = data.get("session_id", "unknown")
    transcript_path = data.get("transcript_path", "")

    transcript_content = read_transcript(transcript_path)
    current_tokens = count_tokens(transcript_content)

    # Save to temp file for later comparison
    state_file = get_state_file(session_id)
    with open(state_file, "w") as f:
        json.dump({"pre_tokens": current_tokens}, f)

def handle_stop(data: dict) -> None:
    """Post-response hook: Calculate and report token delta."""
    session_id = data.get("session_id", "unknown")
    transcript_path = data.get("transcript_path", "")

    transcript_content = read_transcript(transcript_path)
    current_tokens = count_tokens(transcript_content)

    # Load pre-message count
    state_file = get_state_file(session_id)
    pre_tokens = 0
    if os.path.exists(state_file):
        try:
            with open(state_file, "r") as f:
                state = json.load(f)
                pre_tokens = state.get("pre_tokens", 0)
        except (json.JSONDecodeError, IOError):
            pass

    # Calculate delta
    delta_tokens = current_tokens - pre_tokens
    remaining = CONTEXT_LIMIT - current_tokens
    percentage = (current_tokens / CONTEXT_LIMIT) * 100

    # Report usage
    method = "tiktoken" if USE_TIKTOKEN else "estimated"
    print(f"Context ({method}): ~{current_tokens:,} tokens ({percentage:.1f}% used, ~{remaining:,} remaining)", file=sys.stderr)
    if delta_tokens > 0:
        print(f"This request: ~{delta_tokens:,} tokens", file=sys.stderr)

def main():
    data = json.load(sys.stdin)
    event = data.get("hook_event_name", "")

    if event == "UserPromptSubmit":
        handle_user_prompt_submit(data)
    elif event == "Stop":
        handle_stop(data)

    sys.exit(0)

if __name__ == "__main__":
    main()
```

**Configuration:**

```
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/context-tracker.py\""
          }
        ]
      }
    ],
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/context-tracker.py\""
          }
        ]
      }
    ]
  }
}
```

**How it works:**

1. `UserPromptSubmit` fires before your prompt is processed - saves current token count
2. `Stop` fires after Claude responds - calculates delta and reports usage
3. Each session is isolated via in the temp filename `session_id`

**Token Counting Methods:**

| Method | Accuracy | Dependencies | Speed |
| --- | --- | --- | --- |
| Character estimation | ~80-90% | None | <1ms |
| tiktoken (p50k\_base) | ~90-95% | `pip install tiktoken` | <10ms |

> **Note:** Anthropic hasn't released an official offline tokenizer. Both methods are approximations. The transcript includes user prompts, Claude's responses, and tool outputs, but NOT system prompts or internal context.

### Example 7: Seed Auto-Mode Permissions (One-Time Setup Script)

A one-time setup script that seeds with ~67 safe permission rules equivalent to Claude Code's auto-mode baseline — without any hook, without remembering future choices. Run it once; safe to re-run (skips rules already present).`~/.claude/settings.json`

**File:** `09-advanced-features/setup-auto-mode-permissions.py`

```
# Preview what would be added
python3 09-advanced-features/setup-auto-mode-permissions.py --dry-run

# Apply
python3 09-advanced-features/setup-auto-mode-permissions.py
```

**What gets added:**

| Category | Examples |
| --- | --- |
| Built-in tools | `Read(*)`,,,,,, `Edit(*)` `Write(*)` `Glob(*)` `Grep(*)` `Agent(*)` `WebSearch(*)` |
| Git read | `Bash(git status:*)`,, `Bash(git log:*)` `Bash(git diff:*)` |
| Git write (local) | `Bash(git add:*)`,, `Bash(git commit:*)` `Bash(git checkout:*)` |
| Package managers | `Bash(npm install:*)`,, `Bash(pip install:*)` `Bash(cargo build:*)` |
| Build & test | `Bash(make:*)`,, `Bash(pytest:*)` `Bash(go test:*)` |
| Common shell | `Bash(ls:*)`,,,, `Bash(cat:*)` `Bash(find:*)` `Bash(cp:*)` `Bash(mv:*)` |
| GitHub CLI | `Bash(gh pr view:*)`,, `Bash(gh pr create:*)` `Bash(gh issue list:*)` |

**What is intentionally excluded** (never added by this script):

- `rm -rf`,, force push, `sudo` `git reset --hard`
- `DROP TABLE`,, `kubectl delete` `terraform destroy`
- `npm publish`,, production deploys `curl | bash`

### Example 8: Learning Progress Logger (SessionEnd)

Log which modules you studied at the end of each Claude Code session. Progress is stored in — outside the repo, so it survives without being overwritten.`~/.claude-howto-progress.json` `git pull`

**Why `SessionEnd` and not `Stop`?** fires after *every* Claude response. fires once when the session terminates — exactly what you want for an end-of-session diary entry.`Stop` `SessionEnd`

**Why `/dev/tty` for input?** Hook scripts receive the hook JSON payload via, so interactive must use directly to reach the terminal.`stdin` `read` `/dev/tty`

**File:** `06-hooks/session-end.sh`

```
#!/usr/bin/env bash
# SessionEnd hook: prompts for modules worked on, then appends a session record
# to ~/.claude-howto-progress.json for persistent learning progress tracking.

PROGRESS_FILE="$HOME/.claude-howto-progress.json"

# Guard: only run inside this repo
if [[ "$CLAUDE_PROJECT_DIR" != *"claude-howto"* ]] && [[ "$PWD" != *"claude-howto"* ]]; then
  exit 0
fi

if [ ! -f "$PROGRESS_FILE" ]; then
  echo '{"sessions":[]}' > "$PROGRESS_FILE"
fi

DATE=$(date +"%Y-%m-%d")
TIME=$(date +"%H:%M")

echo ""
echo " Which modules did you work on? (e.g. 06,07 or press Enter to skip)"
echo " 01=Slash  02=Memory  03=Skills  04=Subagents  05=MCP"
echo " 06=Hooks  07=Plugins 08=Checkpoints 09=Advanced 10=CLI"
printf " > "
read -r INPUT </dev/tty

if [ -z "$INPUT" ] || [ "$INPUT" = "skip" ]; then
  exit 0
fi

MODULES_JSON=$(echo "$INPUT" | tr ',' '\n' | tr -d ' ' | while read -r m; do
  case "$m" in
    01) echo '"01-slash-commands"' ;;
    02) echo '"02-memory"' ;;
    03) echo '"03-skills"' ;;
    04) echo '"04-subagents"' ;;
    05) echo '"05-mcp"' ;;
    06) echo '"06-hooks"' ;;
    07) echo '"07-plugins"' ;;
    08) echo '"08-checkpoints"' ;;
    09) echo '"09-advanced-features"' ;;
    10) echo '"10-cli"' ;;
    *)  echo "\"$m\"" ;;
  esac
done | paste -sd ',' -)

printf " Notes? (optional, press Enter to skip): "
read -r NOTES </dev/tty

# Pass NOTES as a separate argument so Python handles JSON escaping —
# avoids broken JSON when notes contain quotes or backslashes.
python3 - "$PROGRESS_FILE" "$DATE" "$TIME" "$MODULES_JSON" "$NOTES" <<'PYEOF'
import sys, json

path, date, time_str, modules_raw, notes = sys.argv[1], sys.argv[2], sys.argv[3], sys.argv[4], sys.argv[5]

new_session = {
    "date": date,
    "time": time_str,
    "modules": json.loads(f"[{modules_raw}]") if modules_raw else [],
    "notes": notes,
}

with open(path, 'r') as f:
    data = json.load(f)

data.setdefault('sessions', []).append(new_session)

with open(path, 'w') as f:
    json.dump(data, f, indent=2)
PYEOF

echo " Saved to $PROGRESS_FILE"
```

**Install** — copy the script into the project's hook directory so the path in resolves:`settings.json`

```
mkdir -p .claude/hooks
cp 06-hooks/session-end.sh .claude/hooks/
chmod +x .claude/hooks/session-end.sh
```

**Configuration** (in ):`.claude/settings.json`

```
{
  "hooks": {
    "SessionEnd": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR/.claude/hooks/session-end.sh\""
          }
        ]
      }
    ]
  }
}
```

**Output — `~/.claude-howto-progress.json`:**

```
{
  "sessions": [
    {
      "date": "2026-04-18",
      "time": "14:32",
      "modules": ["06-hooks", "07-plugins"],
      "notes": "Installed first hook, tried pre-commit example"
    }
  ]
}
```

**Key patterns demonstrated:**

| Pattern | Why it matters |
| --- | --- |
| `SessionEnd` event | Fires once on exit — not after every response like `Stop` |
| `read -r INPUT </dev/tty` | Hooks own (JSON payload); use for user input `stdin` `/dev/tty` |
| `$CLAUDE_PROJECT_DIR` | Portable path — never hardcode `/Users/yourname/...` |
| Guard clause at top | Prevents the hook running in unrelated projects if installed globally |
| Store outside the repo | `~/` path survives without overwriting your data `git pull` |

**Companion: visual progress tracker**

For a full checkbox-based UI covering all 10 modules, open the included tracker in your browser:

```
open local-progress/index.html
```

Progress is stored in browser (never written to disk inside the repo). Use the **Export** button to save a snapshot as JSON, and **Import** to restore it.`localStorage`

## Plugin Hooks

Plugins can include hooks in their file:`hooks/hooks.json`

**File:** `plugins/hooks/hooks.json`

```
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "${CLAUDE_PLUGIN_ROOT}/scripts/validate.sh"
          }
        ]
      }
    ]
  }
}
```

**Environment Variables in Plugin Hooks:**

- `${CLAUDE_PLUGIN_ROOT}` \- Path to the plugin directory
- `${CLAUDE_PLUGIN_DATA}` \- Path to the plugin data directory

This allows plugins to include custom validation and automation hooks.

## MCP Tool Hooks

MCP tools follow the pattern:`mcp__<server>__<tool>`

```
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "mcp__memory__.*",
        "hooks": [
          {
            "type": "command",
            "command": "echo '{\"systemMessage\": \"Memory operation logged\"}'"
          }
        ]
      }
    ]
  }
}
```

## Security Considerations

### Disclaimer

**USE AT YOUR OWN RISK**: Hooks execute arbitrary shell commands. You are solely responsible for:

- Commands you configure
- File access/modification permissions
- Potential data loss or system damage
- Testing hooks in safe environments before production use

### Security Notes

- **Workspace trust required:** The and hook output commands now require workspace trust acceptance before they take effect.`statusLine` `fileSuggestion`
- **HTTP hooks and environment variables:** HTTP hooks require an explicit list to use environment variable interpolation in URLs. This prevents accidental leakage of sensitive environment variables to remote endpoints.`allowedEnvVars`
- **Managed settings hierarchy:** The setting now respects the managed settings hierarchy, meaning organization-level settings can enforce hook disablement that individual users cannot override.`disableAllHooks`
- **PowerShell auto-approve (v2.1.119):** PowerShell tool commands can be auto-approved in permission mode, matching Bash. This brings parity for Windows users running Claude Code with PowerShell-backed shell tools.

### Best Practices

| Do | Don't |
| --- | --- |
| Validate and sanitize all inputs | Trust input data blindly |
| Quote shell variables: `"$VAR"` | Use unquoted: `$VAR` |
| Block path traversal (`..`) | Allow arbitrary paths |
| Use absolute paths with `$CLAUDE_PROJECT_DIR` | Hardcode paths |
| Skip sensitive files (,, keys)`.env``.git/` | Process all files |
| Test hooks in isolation first | Deploy untested hooks |
| Use explicit for HTTP hooks `allowedEnvVars` | Expose all env vars to webhooks |

## Debugging

### Enable Debug Mode

Run Claude with debug flag for detailed hook logs:

```
claude --debug
```

### Verbose Mode

Use in Claude Code to enable verbose mode and see hook execution progress.`Ctrl+O`

### Test Hooks Independently

```
# Test with sample JSON input
echo '{"tool_name": "Bash", "tool_input": {"command": "ls -la"}}' | python3 .claude/hooks/validate-bash.py

# Check exit code
echo $?
```

## Complete Configuration Example

```
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/validate-bash.py\"",
            "timeout": 10
          }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR/.claude/hooks/format-code.sh\"",
            "timeout": 30
          },
          {
            "type": "command",
            "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/security-scan.py\"",
            "timeout": 10
          }
        ]
      }
    ],
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/validate-prompt.py\""
          }
        ]
      }
    ],
    "SessionStart": [
      {
        "matcher": "startup",
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR/.claude/hooks/session-init.sh\""
          }
        ]
      }
    ],
    "Stop": [
      {
        "hooks": [
          {
            "type": "prompt",
            "prompt": "Verify all tasks are complete before stopping.",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```

## Hook Execution Details

| Aspect | Behavior |
| --- | --- |
| **Timeout** | 60 seconds default, configurable per command |
| **Parallelization** | All matching hooks run in parallel |
| **Deduplication** | Identical hook commands deduplicated |
| **Environment** | Runs in current directory with Claude Code's environment |

## Troubleshooting

### Hook Not Executing

- Verify JSON configuration syntax is correct
- Check matcher pattern matches the tool name
- Ensure script exists and is executable: `chmod +x script.sh`
- Run to see hook execution logs `claude --debug`
- Verify hook reads JSON from stdin (not command args)

### Hook Blocks Unexpectedly

- Test hook with sample JSON: `echo '{"tool_name": "Write", ...}' | ./hook.py`
- Check exit code: should be 0 for allow, 2 for block
- Check stderr output (shown on exit code 2)

### JSON Parsing Errors

- Always read from stdin, not command arguments
- Use proper JSON parsing (not string manipulation)
- Handle missing fields gracefully

## Installation

### Step 1: Create Hooks Directory

```
mkdir -p ~/.claude/hooks
```

### Step 2: Copy Example Hooks

```
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh
```

### Step 3: Configure in Settings

Edit or with the hook configuration shown above.`~/.claude/settings.json``.claude/settings.json`

## Related Concepts

- **[Checkpoints and Rewind](https://github.com/luongnv89/claude-howto/blob/main/08-checkpoints)** - Save and restore conversation state
- **[Slash Commands](https://github.com/luongnv89/claude-howto/blob/main/01-slash-commands)** - Create custom slash commands
- **[Skills](https://github.com/luongnv89/claude-howto/blob/main/03-skills)** - Reusable autonomous capabilities
- **[Subagents](https://github.com/luongnv89/claude-howto/blob/main/04-subagents)** - Delegated task execution
- **[Plugins](https://github.com/luongnv89/claude-howto/blob/main/07-plugins)** - Bundled extension packages
- **[Advanced Features](https://github.com/luongnv89/claude-howto/blob/main/09-advanced-features)** - Explore advanced Claude Code capabilities

## Additional Resources

- **[Official Hooks Documentation](https://code.claude.com/docs/en/hooks)** - Complete hooks reference
- **[CLI Reference](https://code.claude.com/docs/en/cli-reference)** - Command-line interface documentation
- **[Memory Guide](https://github.com/luongnv89/claude-howto/blob/main/02-memory)** - Persistent context configuration

---

**Last Updated**: May 2, 2026 **Claude Code Version**: 2.1.126 **Sources**:

- [https://code.claude.com/docs/en/hooks](https://code.claude.com/docs/en/hooks)
- [https://code.claude.com/docs/en/changelog](https://code.claude.com/docs/en/changelog)
- [https://github.com/anthropics/claude-code/releases/tag/v2.1.118](https://github.com/anthropics/claude-code/releases/tag/v2.1.118)
- [https://github.com/anthropics/claude-code/releases/tag/v2.1.126](https://github.com/anthropics/claude-code/releases/tag/v2.1.126) **Compatible Models**: Claude Sonnet 4.6, Claude Opus 4.7, Claude Haiku 4.5