# MLLM Obsidian Wiki

一个基于 Obsidian 搭建的个人研究知识库，当前重点用于整理 Karpathy 相关内容，以及 MLLM（多模态大模型）、多模态学习、世界模型方向的学习材料、论文摘要与研究思考。

This repository is an Obsidian-based research wiki for organizing learning materials, paper notes, and research insights around Karpathy-related content, MLLM, multimodal learning, and world models.

## 项目定位 / What this project is

这个仓库不是单纯的“笔记堆积”，而是一个逐步演化的研究知识库，目标是把分散的原始资料转化为：

- 可检索的知识节点
- 可追踪的来源摘要
- 可复用的研究理解
- 可持续扩展的主题网络

The goal is to turn scattered source materials into a reusable knowledge system with linked concepts, source summaries, and structured research notes.

## 当前内容 / Current contents

目前仓库主要包含以下几层：

- `raw/`：原始输入材料，如论文、摘录、转录文本
- `wiki/`：结构化知识层，包括概念、实体、来源摘要与索引
- `insights/`：进一步提炼后的总结、灵感与研究思考
- `skills/`：供 Obsidian 可见的 skill 文档副本
- `commands/`：供 Obsidian 可见的命令说明副本
- `.claude/`：实际生效的 Claude Code skills / commands / settings 配置

## 知识库结构 / Knowledge structure

`wiki/` 是当前最适合公开浏览的核心区域，主要包括：

- `wiki/index.md`：知识库主索引
- `wiki/concepts/`：概念节点
- `wiki/entities/`：工具、模型、数据集等实体节点
- `wiki/sources/`：来源摘要
- `wiki/log.md`：知识库更新记录

示例主题包括：

- 3D Affordance Grounding
- Cross-Modal Integration
- HAMMER
- Qwen2.5-VL
- Obsidian Web Clipper

## 工作流 / Workflow

这个仓库对应的整理流程大致是：

1. 将论文、网页、视频转录等内容放入 `raw/`
2. 提炼来源摘要与关键观点
3. 在 `wiki/` 中沉淀概念、实体与关联关系
4. 在 `insights/` 中记录更高层的综合理解与研究问题
5. 通过 Obsidian 双链逐步形成知识网络

## AI / Agent usage

仓库同时探索了 AI 辅助知识管理工作流，包括：

- 用 `/ingest` 将原始资料整理为结构化笔记
- 用 `/query` 基于本地知识库回答问题
- 用 `/lint` 检查知识库结构健康度
- 通过 Claude Code skills / commands 维护知识库演化

这部分配置的可见副本位于：

- `skills/`
- `commands/`

实际生效配置位于：

- `.claude/skills/`
- `.claude/commands/`

## 如何浏览 / How to explore

如果你是第一次进入这个仓库，建议按下面顺序阅读：

1. `wiki/index.md`
2. `wiki/concepts/`
3. `wiki/entities/`
4. `wiki/sources/`
5. `insights/`

如果你使用 Obsidian 打开本仓库，双链结构会更清晰。

## 适用场景 / Use cases

这个知识库适合用于：

- 论文阅读与摘要整理
- 技术概念对比
- Karpathy 相关内容学习
- MLLM / world model 方向研究积累
- 研究灵感沉淀与后续写作复用

## 说明 / Notes

- 这是一个持续演化中的个人研究知识库，不是完整课程或正式教程。
- 部分目录保留了作者的本地工作流配置，以支持长期维护。
- 为避免泄露本地状态与敏感配置，部分文件不会公开上传。

## License

暂未添加 License；如需复用内容，请先联系作者或等待后续补充。
