---
description: 将 raw/ 中的原始资料摄入并编译到知识库
argument-hint: [path]
---

执行 ingest 工作流。

如果提供参数：
- 仅处理指定路径，例如 `/ingest raw/papers/foo.pdf`

如果不提供参数：
- 扫描 `raw/` 下所有未归档资料并逐个处理

处理要求：
- 优先遵循 `.claude/skills/ingest/SKILL.md`
- 读取 `AGENTS.md` 与当前仓库约定
- 原始资料尽量视为只读输入
- 为每份资料生成摘要/提炼笔记
- 需要时建立相关双链
- 若发现知识冲突，先暂停并询问用户
