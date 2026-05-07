# Wiki 操作日志

---

## [2026-05-03] sync | 初始化当前 mllm/wiki 目录结构

- **变更**: 新增 `wiki/concepts/`, `wiki/entities/`, `wiki/sources/`, `wiki/syntheses/`; 创建 [[index.md]] 与 [[log.md]]
- **说明**: 参考 `karpathy-llm-wiki-vault/wiki` 的组织方式，为当前 mllm 仓库建立类似但更适合现有工作流的 wiki 层
- **冲突**: 无

---

## [2026-05-06] ingest | 摄入 2 份原始资料

- **变更**:
  - 新增 Sources: [[摘要-hammer-3d-affordance-grounding]], [[摘要-obsidian-web-clipper]]
  - 新增 Entities: [[HAMMER]], [[Qwen2.5-VL]], [[PIAD]], [[Obsidian Web Clipper]], [[Defuddle]]
  - 新增 Concepts: [[3D Affordance Grounding]], [[Cross-Modal Integration]], [[Multi-Granular Geometry Lifting]], [[AI Interpreter]], [[Web Clipper Workflow]]
  - 更新 [[index.md]]
- **归档**: `raw/papers/2603.02329v1.pdf` → `raw/archive/`; `raw/transcripts/This Obsidian Plugin Just Fixed the Internet 1.md` → `raw/archive/`
- **冲突**: 无
