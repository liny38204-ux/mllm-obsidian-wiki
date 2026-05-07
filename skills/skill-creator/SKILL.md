---
name: skill-creator
description: "Create or optimize Claude Code skills with best-practice structure, bundled resources, and prompt engineering patterns. / 用最佳实践结构、配套资源与提示工程模式创建或优化 Claude Code skills。"
license: Complete terms in LICENSE.txt
---

# Skill Creator / Skill 创建器

按照下述流程创建高质量的 Claude Code skill。关于 skill 结构与配套资源，请阅读 `references/skill-anatomy.md`。

Create high-quality Claude Code skills by following the process below. For skill anatomy and bundled resources, read `references/skill-anatomy.md`.

## Quick Reference / 快速参考

| Step / 步骤 | What / 内容 | When to Skip / 何时可跳过 |
|------|------|-------------|
| 1. Understand / 理解 | Gather concrete usage examples / 收集具体使用示例 | Usage patterns already clear / 使用模式已经清楚 |
| 2. Plan / 规划 | Identify reusable scripts, references, assets / 识别可复用的脚本、参考资料与资源 | Simple skill with no resources / 简单 skill 且无需额外资源 |
| 3. Initialize / 初始化 | Run `init_skill.py` to scaffold / 运行 `init_skill.py` 生成骨架 | Skill already exists / skill 已存在 |
| 4. Edit / 编写 | Write SKILL.md + bundled resources / 编写 SKILL.md 与配套资源 | Never — always needed / 不建议跳过 |
| 5. Package / 打包 | Validate and zip for distribution / 校验并打包为 zip | Skill is local-only / 仅本地使用 |
| 6. Iterate / 迭代 | Test on real tasks, refine / 在真实任务中测试并优化 | Never — always valuable / 不建议跳过 |

创建或修改 skill 后，应用下方的 **Optimization Principles / 优化原则** 来保证质量。

After creating or editing a skill, apply the **Optimization Principles** below to ensure quality.

---

## Skill Creation Process / Skill 创建流程

### Step 1: Understanding the Skill with Concrete Examples / 用具体示例理解 Skill

先明确 skill 将如何被使用。这个理解可以来自用户直接提供的示例，也可以来自你生成并经用户确认的示例。

First understand clearly how the skill will be used. This understanding can come from direct user examples or generated examples validated with user feedback.

例如，在构建一个 image-editor skill 时，可以问：

For example, when building an image-editor skill, ask questions such as:

- "What functionality should the image-editor skill support?" / “image-editor skill 需要支持哪些功能？”
- "Can you give some examples of how this skill would be used?" / “能举几个这个 skill 的使用例子吗？”
- "What would a user say that should trigger this skill?" / “用户通常会怎么说，才应该触发这个 skill？”

优先问最关键的问题，并按需要追问；当功能边界已经清楚时结束这一阶段。

Start with the most important questions and follow up only as needed. Conclude when the intended functionality is clear.

### Step 2: Planning the Reusable Skill Contents / 规划可复用内容

对每个具体示例进行分析：

Analyze each concrete example by:

1. 思考如果从零执行这个示例，需要做什么。 / Considering how to execute the example from scratch.
2. 识别哪些脚本、参考资料、资源有助于反复执行这类流程。 / Identifying which scripts, references, and assets would help repeat the workflow.

关于何时使用 `scripts/`、`references/`、`assets/`，请阅读 `references/skill-anatomy.md`。

For guidance on when to use `scripts/`, `references/`, and `assets/`, read `references/skill-anatomy.md`.

### Step 3: Initializing the Skill / 初始化 Skill

从零创建新 skill 时，运行初始化脚本：

When creating a new skill from scratch, run the init script:

```bash
python ~/.claude/skills/skill-creator/scripts/init_skill.py <skill-name> --path <output-directory>
```

该脚本会创建 skill 目录、SKILL.md 模板、正确的 frontmatter，以及示例性的 `scripts/`、`references/`、`assets/` 目录。初始化后，按需修改或删除示例文件。

The script creates the skill directory with a SKILL.md template, proper frontmatter, and example `scripts/`, `references/`, and `assets/` directories. After initialization, customize or remove the generated example files.

如果 skill 已存在，只是需要继续迭代或打包，可以跳过这一步。

Skip this step if the skill already exists and iteration or packaging is all that is needed.

### Step 4: Edit the Skill / 编写 Skill

这个 skill 是给另一个 Claude 实例使用的，所以重点写“有帮助且不显然”的信息。

The skill is being created for another Claude instance to use, so focus on information that is beneficial and non-obvious.

#### Start with Reusable Skill Contents / 先实现可复用资源

先实现第 2 步中识别出的 `scripts/`、`references/`、`assets/` 文件。这可能需要用户提供输入，例如品牌资源、API 文档等。不需要的示例文件应删除。

Implement the `scripts/`, `references/`, and `assets/` files identified in Step 2. This may require user input such as brand assets or API documentation. Delete any example files that are not needed.

#### Write SKILL.md / 编写 SKILL.md

**Writing Style / 写作风格：** 使用 **祈使式 / 不定式**（动词开头的指令风格），不要用第二人称。保持客观、可执行的说明语言，例如用 “To accomplish X, do Y” 而不是 “You should do X”。

**Writing Style:** Use **imperative/infinitive form** (verb-first instructions), not second person. Use objective, instructional language.

在 SKILL.md 中回答以下问题：

Answer these questions in SKILL.md:

1. 这个 skill 的目的是什么？用几句话说清楚。 / What is the purpose of the skill, in a few sentences?
2. 什么时候应该使用这个 skill？ / When should the skill be used?
3. Claude 应该怎样使用它？把所有配套资源都点出来。 / How should Claude use it? Reference all bundled resources so Claude knows they exist.

### Step 5: Packaging a Skill / 打包 Skill

将 skill 打包为可分发的 zip：

Package the skill into a distributable zip:

```bash
python ~/.claude/skills/skill-creator/scripts/package_skill.py <path/to/skill-folder>
```

该脚本会先做校验（frontmatter、命名、目录结构、description 质量），校验通过后再打包。若校验失败，先修复问题再重新运行。

The script validates frontmatter, naming, structure, and description quality, then packages if validation passes. Fix any errors and re-run if validation fails.

### Step 6: Iterate / 迭代

**迭代流程：**
1. 在真实任务中使用这个 skill。 / Use the skill on real tasks.
2. 观察它在哪些地方吃力或低效。 / Notice struggles or inefficiencies.
3. 判断 SKILL.md 或配套资源该如何更新。 / Identify how SKILL.md or bundled resources should be updated.
4. 实施修改并再次测试。 / Implement changes and test again.

---

## Optimization Principles / 优化原则

在创建非平凡 skill 或迭代现有 skill 时，应用这些原则。它们来自生产环境中的 skill 优化经验，能直接改善 LLM 的行为表现。

Apply these when creating non-trivial skills or iterating on existing ones. These patterns are derived from production skill optimization and directly improve LLM behavior.

### 1. Information Architecture: Position for LLM Attention / 信息架构：为 LLM 注意力安排位置

LLM 会按顺序读取 SKILL.md，而且越靠前的内容通常权重越高。

An LLM reads SKILL.md sequentially and usually weights earlier content more heavily.

- **规则和约束放在参考资料前面。** 行为规则必须先于被约束内容出现；如果流程在前、规则在后，模型在执行时可能忘掉规则。
  **Rules and constraints before reference material.** Behavioral rules must appear before the content they constrain.
- **把每次调用都需要的信息前置。** 分类、模式选择、全局规则应尽早出现；条件性内容可以靠后或拆到独立文件。
  **Front-load what is needed on every invocation.** Put classification, mode selection, and global rules early.
- **把背景资料移到 `references/`。** 不需要每次调用都读的背景信息、结构说明、示例，放到参考文件而不是 SKILL.md 里。
  **Move reference material to `references/`.** Put background information not needed every time into reference files.

### 2. The Router Pattern: Modular Skills / Router 模式：模块化 Skill

当一个 skill 有 3 条以上明显不同的执行路径，并且总长度超过约 400 行时，将它拆成 router + 分路径文件：

When a skill has 3+ distinct code paths and exceeds roughly 400 lines, split it into a router plus per-path files:

```
skill-name/
├── SKILL.md              ← Router: classification + shared rules (~200 lines)
└── workflows/
    ├── path-a.md         ← Loaded on demand after classification
    ├── path-b.md
    └── path-c.md
```

这种模式通常能把初始上下文减少 40–70%。代价是每次调用多一次 `Read` 工具调用（约 1 秒）。建议在路由表中增加 “File” 列，让 LLM 明确知道要读哪个文件。

This pattern often reduces initial context by 40–70%. The trade-off is one extra `Read` tool call per invocation (about one second). Add a “File” column to routing tables so the LLM has an explicit path to `Read`.

### 3. Classification Design: Priority + Disambiguation / 分类设计：优先级 + 消歧

当 skill 需要把用户输入分类到不同类别时：

When a skill classifies user input into categories:

1. **定义严格的优先级顺序**，用来确定多重匹配时的唯一结果。 / **Define a strict priority order** to resolve multi-match cases deterministically.
2. **加入消歧规则**，处理“看起来像 A 其实是 B”的模式。 / **Add disambiguation rules** for patterns that look like one type but are actually another.
3. **用 20–30 条对抗样例测试**，覆盖清晰样例、模糊多匹配、以及误报陷阱。 / **Test with 20–30 adversarial inputs** including clear cases, ambiguous multi-match, and false-positive traps.

### 4. Inline References Beat Cross-References / 行内提醒优于跨段引用

如果某一处定义的规则需要在别处执行，那么就在执行点再写一次简短提醒。不要假设 LLM 会在几百行内容之间可靠地回跳交叉引用。适度冗余比省字更可靠。

When a rule defined in one section must be applied elsewhere, add an inline reminder at the point of use. Do not rely on the LLM to cross-reference hundreds of lines. Some redundancy produces more reliable behavior.

### 5. Prompt Engineering Checklist / 提示工程检查清单

每个 skill 在交付前都检查：

Apply this checklist before shipping every skill:

- [ ] **Frontmatter description 控制在 120 字符以内**——过长会影响发现与匹配  
      **Keep the frontmatter description under 120 characters** — long descriptions hurt discovery
- [ ] **为路由或多步骤流程提供 Quick Reference 表**——表格对 LLM 来说是 O(1) 访问  
      **Include a Quick Reference table** for routing or multi-step processes — tables are O(1) for LLMs
- [ ] **在高风险场景写出明确的 “Do NOT” 禁令**——防止出于好意的破坏性操作  
      **Use explicit “Do NOT” prohibitions** in high-stakes situations — this prevents well-intentioned destructive behavior
- [ ] **区分 optional 与 required 依赖**——可选依赖失败可跳过，必需依赖失败应重试后再问用户  
      **Categorize optional vs. required dependencies** — optional skills skip on failure; required skills retry then ask the user
- [ ] **把背景/参考资料放入 `references/`**，不要全部内联在 SKILL.md 中  
      **Keep background/reference material in `references/`** rather than inline in SKILL.md
- [ ] **脚本引用使用绝对路径**——LLM 需要完整路径，不要只给相对路径  
      **Use absolute paths for script references** — the LLM needs the full path, not a relative one
