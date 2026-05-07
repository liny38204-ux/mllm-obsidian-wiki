---
title: "AI Interpreter"
type: concept
tags: [概念, AI, Obsidian, 摘要生成, 工具]
sources: [raw/transcripts/This Obsidian Plugin Just Fixed the Internet 1.md]
last_updated: 2026-05-06
---

## 定义

AI Interpreter 是 Obsidian Web Clipper 内置的可选 AI 层，用于为抓取的网页或视频转录内容自动生成三层结构化摘要。它是模型无关的（model-agnostic），用户可自由选择 Claude、GPT、Gemini、DeepSeek 或本地 Ollama 模型。

## 关键信息

- **三层摘要结构**：
  1. **Summary**：清洁、简洁的纯文本总结段落
  2. **Headlines**：简短的关键要点列表
  3. **Things**：文章中提到的关键人物、术语、想法的热列表
- **设计哲学**：解决传统"概括此内容"提示导致的结果过长、模糊或不一致的问题。三层结构让用户能在 5 秒内扫描总结、15 秒内读标题，或深入所有关联事物
- **配置**：通过 Web Clipper Settings → Interpreter 启用并配置 AI 提供商和模型
- **优势**：当更好的模型出现时，只需切换即可，无需改变工作流

## 关联连接

- [[Obsidian Web Clipper]] — 所属工具
- [[Web Clipper Workflow]] — 相关概念
- [[摘要-obsidian-web-clipper]] — 来源摘要
