# LLM Wiki 集成方案

## 摘要

已将 `karpathy-llm-wiki-vault` clone 到当前 Obsidian 仓库内，路径为：

- `/Users/lingao/mllm/karpathy-llm-wiki-vault`

为了避免大规模改动你现有的知识库结构，这里采用**非破坏式整合**：

- 保留你当前的 `raw/`、`insights/`、`user/` 结构
- 将 `karpathy-llm-wiki-vault` 作为一个可直接浏览和参考的子目录
- 先通过说明文档建立映射关系，后续再按需要逐步吸收其方法

## 当前状态

你的当前 vault 主要结构：

- `raw/`：原始材料
- `insights/`：研究灵感与总结
- `user/`：个人相关笔记
- `未命名/`：临时笔记
- `karpathy-llm-wiki-vault/`：新引入的 LLM Wiki 模板仓库

引入仓库的主要结构：

- `karpathy-llm-wiki-vault/raw/`
- `karpathy-llm-wiki-vault/wiki/`
- `karpathy-llm-wiki-vault/assets/`
- `karpathy-llm-wiki-vault/CLAUDE.md`

## 推荐的对应关系

建议先按下面的方式理解两套结构：

### 1. 原始资料层

- 你现在的 `raw/`
- 对应模板中的 `karpathy-llm-wiki-vault/raw/`

含义：
- 都是原始输入区
- 适合放论文、网页摘录、视频转录、会议记录

建议：
- 你现阶段继续优先使用你自己的 `raw/`
- 如果后续你希望采用更细分类，可以逐步参考模板拆成：
  - `01-articles/`
  - `02-papers/`
  - `03-transcripts/`
  - `04-meeting_notes/`
  - `09-archive/`

### 2. 提炼/知识层

- 你现在的 `insights/`
- 对应模板中的 `karpathy-llm-wiki-vault/wiki/`

含义：
- 都承担“从材料到知识”的整理工作
- 模板把知识层进一步拆成：
  - `concepts/`：概念、方法、原理
  - `entities/`：人物、项目、工具、组织
  - `sources/`：对单个原始材料的摘要
  - `syntheses/`：面向问题的综合报告

建议：
- 你现阶段继续使用 `insights/`
- 当某一类笔记开始变多时，再考虑把 `insights/` 细分成类似 `concepts/`、`sources/`、`syntheses/`

### 3. 个人信息层

- 你现在的 `user/`
- 模板里没有专门对应层

建议：
- 保持不变
- 继续将和身份、背景、研究方向相关的内容放在 `user/`

## 最小整合工作流

在不重构现有仓库的前提下，可以先这样使用：

1. 新论文/网页/视频材料先进入你自己的 `raw/`
2. 阅读后把总结写到 `insights/`
3. 需要参考 LLM Wiki 方法时，查看下面这些文件：
   - [模板说明 README](../karpathy-llm-wiki-vault/README.md)
   - [模板规则 CLAUDE](../karpathy-llm-wiki-vault/CLAUDE.md)
   - [模板知识索引](../karpathy-llm-wiki-vault/wiki/index.md)
   - [模板日志](../karpathy-llm-wiki-vault/wiki/log.md)
4. 当你确认某种结构长期有用，再迁移到主仓库中

## 我建议你下一步优先吸收的部分

相比直接整体搬运，更建议优先吸收这些方法：

- **raw 和知识层分离**：原始输入与整理输出分开
- **source note 思路**：每篇论文/文章单独提炼一页
- **synthesis note 思路**：围绕一个研究问题做跨来源综合
- **index/log 思路**：给知识库做可追踪索引与操作日志

## 一个适合你当前仓库的渐进式升级方案

### Phase 1：保持现状，只增加方法

不移动文件，只增加以下习惯：

- 每篇论文对应一个摘要笔记
- 每个研究主题对应一个综合笔记
- 多使用笔记之间的链接

### Phase 2：轻量结构化

如果 `insights/` 内容增多，可以新增：

- `insights/concepts/`
- `insights/sources/`
- `insights/syntheses/`

### Phase 3：整理 raw

如果 `raw/` 内容增多，可以新增：

- `raw/articles/`
- `raw/papers/`
- `raw/transcripts/`
- `raw/archive/`

## 备注

目前我没有移动、合并或删除你现有的任何笔记，只是：

- clone 了模板仓库
- 在你的 `insights/` 下创建了这份整合说明

这样做的好处是：

- 安全
- 可回退
- 不打断你现有工作流

## Related notes

- [模板说明 README](../karpathy-llm-wiki-vault/README.md)
- [模板规则 CLAUDE](../karpathy-llm-wiki-vault/CLAUDE.md)
