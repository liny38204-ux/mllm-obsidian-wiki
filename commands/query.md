---
description: 在当前知识库中检索并回答问题
argument-hint: <question>
---

执行 query 工作流来回答用户问题：$ARGUMENTS

要求：
- 优先遵循 `.claude/skills/query/SKILL.md`
- 先查看已有索引、相关笔记和上下文，再回答
- 回答时优先使用 Obsidian 双链引用
- 如果本地知识库没有相关内容，明确说明，再补充通用回答
