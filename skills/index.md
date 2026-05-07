# Skills 索引 / Skills Index

## 摘要 / Summary

这个页面是 `skills/` 目录的主入口，用于在 Obsidian 中浏览当前可见的 skills 副本、理解目录结构，并快速定位每个 skill 的用途。

This page is the main entry for the `skills/` directory. It helps browse the visible skill copies in Obsidian, understand the folder structure, and quickly locate what each skill is for.

真实生效的位置仍然是：

The real runtime location is still:

- `.claude/skills/`

`skills/` 目录中的内容主要用于阅读、整理和分享，不会自动回写到真实配置目录。

The contents in `skills/` are mainly for reading, organizing, and sharing. They do not automatically sync back to the real configuration directory.

## 目录结构 / Directory Structure

```text
└── 🛠️ skills/                        ← Agent Skill 中心（Obsidian 可见副本）
    ├── 📘 index.md                   ← 本目录的主入口与总览页
    ├── 📄 README.md                  ← 兼容旧入口的跳转说明
    ├── ⚙️ ingest/                    ← 自定义：将 raw/ 原始资料整理进 wiki
    │   └── SKILL.md                  ← ingest 的主说明文件
    ├── 🔎 query/                     ← 自定义：检索 wiki/index 与相关页面并回答问题
    │   └── SKILL.md                  ← query 的主说明文件
    ├── 🩺 lint/                      ← 自定义：检查 wiki 死链、孤儿页面与索引一致性
    │   └── SKILL.md                  ← lint 的主说明文件
    └── 🪄 skill-creator/             ← Claude Code skill 创建与优化工具箱
        ├── SKILL.md                  ← skill-creator 的主说明文件
        ├── references/               ← 按需读取的结构参考资料
        │   └── skill-anatomy.md      ← skill 目录结构与资源组织说明
        └── scripts/                  ← 供创建/打包 skill 复用的辅助脚本
```

### 结构说明 / Structure Notes

- `index.md`：本目录的推荐导航入口
- `README.md`：兼容旧入口的说明页
- `*/SKILL.md`：对应 skill 的主说明文件
- `references/`：按需读取的参考资料
- `scripts/`：供 skill 调用或复用的辅助脚本

- `index.md`: recommended navigation entry for this directory
- `README.md`: compatibility page for the previous entry point
- `*/SKILL.md`: main instruction file for each skill
- `references/`: reference materials loaded on demand
- `scripts/`: helper scripts used or reused by a skill

## Skills 简要总结 / Skills at a Glance

- [[ingest/SKILL]] — 将 `raw/` 下的原始资料整理为结构化知识笔记，并更新 `wiki/index.md` 与 `wiki/log.md`
- [[query/SKILL]] — 从 `wiki/index.md` 起步检索本地知识库，输出带双链引用的回答
- [[lint/SKILL]] — 扫描 `wiki/` 健康状况，识别死链、孤儿页面、未同步索引与知识冲突
- [[skill-creator/SKILL]] — 创建或优化 Claude Code skill，并组织 `SKILL.md`、`references/`、`scripts/` 等配套资源

## 关键参考 / Key References

- [[skill-creator/SKILL]] — skill 编写与优化流程
- [[skill-creator/references/skill-anatomy]] — skill 目录解剖、资源分类与最佳实践

- [[skill-creator/SKILL]] — workflow for writing and optimizing skills
- [[skill-creator/references/skill-anatomy]] — skill anatomy, bundled resource types, and best practices

## 真实路径对照 / Runtime Path Mapping

- `skills/ingest/SKILL.md` → `.claude/skills/ingest/SKILL.md`
- `skills/query/SKILL.md` → `.claude/skills/query/SKILL.md`
- `skills/lint/SKILL.md` → `.claude/skills/lint/SKILL.md`
- `skills/skill-creator/SKILL.md` → `.claude/skills/skill-creator/SKILL.md`

## 维护说明 / Maintenance Notes

- 如果修改了 `.claude/skills/` 下的真实文件，建议同步更新 `skills/` 中的可见副本
- 如果后续 skill 数量继续增加，可以把本页扩展成按主题分组的目录页

- If files under `.claude/skills/` are updated, the visible copies under `skills/` should be synchronized as well
- If more skills are added later, this page can grow into a topic-grouped directory page
