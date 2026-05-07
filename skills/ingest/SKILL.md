---
name: ingest
description: 将 raw/ 目录下的原始资料整理为结构化知识笔记。支持 `/ingest`（扫描 raw/ 下所有未归档文件）或 `/ingest <path>`（处理指定文件）。处理完成后，将源文件移动到 raw/archive/ 归档，并绝对忽略 archive 目录。
user-invocable: true
---

# ingest 技能 / Ingest Skill

## 核心工作流：Inbox & Archive / Core Workflow: Inbox & Archive

你正在维护一个 Obsidian 知识库。`raw/` 是待处理输入层，`wiki/` 是结构化知识层，`insights/` 是提炼与研究思考层。

You are maintaining an Obsidian knowledge vault. `raw/` is the intake layer, `wiki/` is the structured knowledge layer, and `insights/` is the distilled research-thinking layer.

**目录结构约定 / Directory conventions:**
- `raw/articles/` — 网页剪藏、文章类 Markdown 输入  
  `raw/articles/` — clipped web articles and article-style Markdown inputs
- `raw/papers/` — 论文、PDF、文献类输入  
  `raw/papers/` — papers, PDFs, and literature inputs
- `raw/transcripts/` — 视频转录、讲座记录、字幕文稿  
  `raw/transcripts/` — video transcripts, lecture notes, and subtitle text
- `raw/archive/` — 已处理文件的归档目录，禁止读取  
  `raw/archive/` — archive directory for processed files; do not read from it
- `wiki/sources/` — 资料摘要  
  `wiki/sources/` — source summaries
- `wiki/entities/` — 实体（人物、公司、工具、产品）  
  `wiki/entities/` — entities such as people, companies, tools, and products
- `wiki/concepts/` — 概念（框架、方法论、理论）  
  `wiki/concepts/` — concepts such as frameworks, methodologies, and theories


## 触发逻辑 / Trigger logic

1. **用户执行 `/ingest`**：扫描 `raw/` 下所有未归档文件，找出待处理资料。  
   **User runs `/ingest`**: scan all unarchived files under `raw/` and identify pending materials.
2. **用户执行 `/ingest <path>`**：仅处理指定文件。  
   **User runs `/ingest <path>`**: process only the specified file.
3. **隐式触发**：用户说“把这个资料摄入知识库”“导入这篇文章”“整理这个转录稿”时，自动执行 ingest。  
   **Implicit trigger**: automatically invoke ingest when the user says things like “ingest this material”, “import this article”, or “organize this transcript”.

如果指定路径位于 `archive/` 目录中，应拒绝重复处理，并明确说明该文件已归档。

If the specified path is inside an `archive/` directory, refuse re-processing and clearly explain that the file has already been archived.

## 编译流水线 / Compilation pipeline

对每个待处理源文件，严格按以下步骤执行：

For each pending source file, follow the steps below strictly.

### 步骤 1：读取源文件 / Step 1: Read the source file

- **如果是 `.md` 文件**：使用读取工具完整读取内容。  
  **For `.md` files**: read the full contents with the read tool.
- **如果是 `.pdf` 文件**：使用读取工具尝试提取文本。如果无法提取或内容为空，改为记录文件元信息（文件名、页数）在 sources 页面中。  
  **For `.pdf` files**: attempt text extraction with the read tool. If extraction fails or the content is empty, record file metadata such as filename and page count in the source page instead.

### 步骤 2：提炼核心并翻译 / Step 2: Extract core ideas and translate

从源文件中提取：

Extract the following from the source file:

- **核心主旨**：这段资料讲什么（1-2 句话）  
  **Core thesis**: what this material is about in 1–2 sentences
- **实体**：人物、公司、工具、产品等具体名词  
  **Entities**: concrete nouns such as people, companies, tools, and products
- **概念**：框架、方法论、理论等抽象名词  
  **Concepts**: abstract notions such as frameworks, methodologies, and theories

如果是非中文内容，则翻译成中文。

If the source is not in Chinese, translate it into Chinese.

### 步骤 3：创建来源摘要 / Step 3: Create a source summary

在 `wiki/sources/` 创建 Markdown 文件：

Create a Markdown file under `wiki/sources/`:

```markdown
---
title: "摘要-文件slug"
type: source
tags: [来源, 原始文件]
sources: [raw/articles/xxx.md]
last_updated: YYYY-MM-DD
---

## 核心摘要
[3-5 句话的核心总结]

## 关联连接
- [[EntityName]] — 关联实体
- [[ConceptName]] — 关联概念
```

`sources` 字段应记录源文件的真实相对路径，不要强行套用固定编号目录。

The `sources` field should store the real relative path of the source file. Do not force it into a numbered-folder pattern.

文件名使用 kebab-case：`摘要-{文件slug}.md`

Use kebab-case for filenames: `摘要-{文件slug}.md`.

### 步骤 4：知识网络化（实体/概念页面） / Step 4: Build the knowledge graph (entity/concept pages)

对于步骤 2 提取的每个实体和概念：

For each entity and concept extracted in Step 2:

**目标目录 / Target directories:**
- 实体 → `wiki/entities/`  
  Entities → `wiki/entities/`
- 概念 → `wiki/concepts/`  
  Concepts → `wiki/concepts/`

**处理逻辑 / Processing logic:**
1. 页面不存在 → 按照 `CLAUDE.md` 的约定创建新页面  
   If the page does not exist, create it according to `CLAUDE.md` conventions.
2. 页面已存在 → 读取现有内容，**增量合并**新信息  
   If the page already exists, read the current content and **merge incrementally**.
3. **发现冲突** → **立即暂停**，向用户报告冲突内容，询问处理方式后再继续  
   If a **conflict** is found, **pause immediately**, report the conflict to the user, and continue only after they choose how to resolve it.

**页面模板 / Page template:**

```markdown
---
title: "页面名称"
type: entity | concept
tags: [标签]
sources: [关联的源文件]
last_updated: YYYY-MM-DD
---

## 定义
[对该实体/概念的定义]

## 关键信息
[从源文件中提取的详细信息]

## 关联连接
- [[摘要-source-slug]] — 来源
- [[RelatedEntity]] — 相关实体
```

### 步骤 5：更新全局注册表 / Step 5: Update global registries

**更新 `wiki/index.md`：**  
按照 `CLAUDE.md` 规定的格式，将新增页面添加到对应分类下：
- Sources: `[[摘要-source-slug]] — 该资料的核心主旨`
- Entities: `[[EntityName]] — 该实体的身份定义`
- Concepts: `[[ConceptName]] — 该概念的核心定义`

**Update `wiki/index.md`:**  
Add new pages to the appropriate sections using the format defined in `CLAUDE.md`.

**更新 `wiki/log.md`：**  
追加操作日志（append-only）：

**Update `wiki/log.md`:**  
Append an operation log entry (append-only):

```markdown
## [YYYY-MM-DD] ingest | 操作简述
- **变更**: 新增 [[PageName]]; 更新 [[index.md]]
- **冲突**: 无 (或: 冲突 [[ConflictingPage]], 已暂停等待决策)
```

### 步骤 6：归档源文件 / Step 6: Archive the source file

在确认以下全部完成后，将源文件移动到 `raw/archive/`：
- sources 页面已创建
- 实体/概念页面已创建或更新
- index.md 已更新
- log.md 已更新

After confirming all of the following are complete, move the source file to `raw/archive/`:
- the source page has been created
- entity/concept pages have been created or updated
- `index.md` has been updated
- `log.md` has been updated

**绝对禁止修改源文件内部的文字。**  
**Never modify the internal text of the source file itself.**

## 冲突处理流程 / Conflict handling flow

当发现新旧知识冲突时：

When a conflict between new and existing knowledge is found:

1. **暂停**：停止当前 ingest 流程  
   **Pause**: stop the current ingest flow.
2. **报告**：向用户说明冲突内容（哪个页面、冲突点是什么）  
   **Report**: explain which page conflicts and what the conflict is.
3. **询问**：请用户选择处理方式：  
   **Ask** the user to choose one of the following:
   - A) 保留新旧两者，标注为“知识冲突”  
     Keep both versions and label them as a knowledge conflict
   - B) 用新知识覆盖旧知识  
     Replace the old knowledge with the new one
   - C) 放弃本次 ingest  
     Abort this ingest run
4. **继续**：根据用户选择继续或终止  
   **Continue**: proceed or terminate based on the user's choice.

## 注意事项 / Notes

- 绝对不读取任何位于 `archive/` 路径下的文件  
  Never read files located under any `archive/` path
- 所有 wiki 页面必须包含 `## 关联连接` 区域，不能产生孤岛页面  
  All wiki pages must include a `## 关联连接` section to avoid isolated pages
- 使用简体中文编写所有内容  
  Write all generated content in Simplified Chinese
- 实体命名使用 TitleCase，概念和来源使用 kebab-case  
  Use TitleCase for entity names and kebab-case for concepts and source filenames
- 目录名只作为语义提示，不应成为是否可 ingest 的硬前提  
  Directory names are semantic hints only and must not become hard prerequisites for ingest eligibility
