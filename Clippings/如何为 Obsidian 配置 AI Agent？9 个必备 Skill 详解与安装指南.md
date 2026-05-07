---
title: "如何为 Obsidian 配置 AI Agent？9 个必备 Skill 详解与安装指南"
source: "https://www.youtube.com/watch?v=ivApMNUlmWg"
author:
  - "[[杰森的效率工坊]]"
published: 2026-04-03
created: 2026-04-30
description: "用 AI Agent来接管 Obsidian 知识库绝对是未来的趋势，但如果你闭着眼睛乱选 Skill，不仅完不成任务，还会白白烧掉大量的 Token。这个视频我就带你彻底了解 Obsidian 最核心的 9 个 Agent Skill，认清 OpenClaw 官方仓库的Obsidian Skill的过时问题，以..."
tags:
  - "clippings"
---
![](https://www.youtube.com/watch?v=ivApMNUlmWg)

## Transcript

### 别给你的Obsidian乱装Skill

**0:00** · 今天来为大家分享obsidian 用户必备的9个agent skills 以及它们的使用方法 在这个智能体时代 skill市场鱼龙混杂 哪怕是obsidian CEO发布的skills 也并不一定每个都有重要作用 而Openclaw大龙虾官方GitHub仓库中 的obsidian skill甚至是一个过时的

**0:19** · 理应被淘汰的skill 如果你对这些skill的理解不够透彻 很容易影响到你的日常工作和学习 还会导致智能体额外消耗大量token 却完成不了任务 今天我就和大家逐一分析 哪些skill对于obsidian 用户来说是核心必备 哪些skill需要淘汰 我主要为大家介绍以下几个skill 首先是Obsidian的CEO steph发布的5个skill

### 视频内容介绍

**0:43** · 其中json-canvas这个skill有更好的替代 而Obsidian Markdown这个skill 我们可以进行一些扩展 然后我们来看一下为什么 Openclaw大龙虾官方仓库里的 obsidian skill不适合我们使用 第三我来介绍著名博主Axton 发布的三个画图skill 分别是json-canvas excalidraw和Mermaid 最后 我们来看两个做深度知识学习的skill 这两个skill都能够直接把你想要学习 的文档资料转换成完整的obsidian知识库

**1:15** · 带有双链和MOC内容地图 那这两个skill是我重点推荐的skill 非常的有价值 视频最后 我还会快速地介绍一下两个obsidian 智能体插件的安装和配置方法 并留给大家一个开放性的问题来思考 今天的内容我都整理成了知识笔记 视频的最后会分享给大家 那我们就正式开始 首先我们来看obsidian CEO steph发布的skills

**1:39** · 在GitHub上搜索kepano 这个名字就是steph的网名 他的推特也是这个名字 那著名的obsidian主题 皮肤Minimal就是他写的 在他的GitHub仓库中 找到obsidian skills这个仓库 啊可以看到目前一共有5个skill 我们逐一来进行分析 首先defuddle skill 这个是核心必备 为什么呢 obsidian官方剪藏插件Web Clipper

### Defuddle Skill：网页与视频剪藏

**2:04** · 就是基于defuddle这个工具 defuddle是一个命令行工具 那也是steph写的 在它的仓库里也可以找到 那这个工具是一个网页剪藏工具 可以把你浏览的网页 转换成Markdown格式的内容 非常适合你在浏览网络文章的时候 把知识一键保存到你的知识库中

**2:22** · 而最近的一次更新 defuddle还支持了Youtube链接 输入YouTube视频链接 就可以一键提取视频文案 并转换成Markdown格式 它和我之前视频中给 大家展示的工具不同 它使用的是YouTube官方api 而不是yt-dlp工具 相对来说更稳定 也不需要配置cookie 所以defuddle skill是obsidian用户必备的skill 那使用方法也很简单 先通过命令行npm install -g defuddle

**2:49** · 来安装defuddle这个工具 然后把这个skill放到你的 智能体文件夹里就可以了 那如何安装skill 我们已经讲了很多遍了 就不再重复了 那使用方法非常简单 直接发送网页链接 让智能体转换成知识笔记就可以了 我直接给大家展示两个Markdown笔记 首先第一个是网络文章的剪藏

**3:10** · 我使用的是谷歌的 开发者blog中的一篇文章 我把文章的链接发送给智能体 让它读取文章内容并进行翻译 然后生成Markdown笔记 那可以看到生成的笔记 可以说是非常的完美 把网页中的图片和 代码块都保存了下来 那另一个是Youtube视频的转录 我使用的是著名博主Dan Koe的视频 那视频转录成 Markdown文档后 包含了详细的元数据信息 视频链接视频内容的分段总结

**3:39** · 那你当然也可以使用 obsidian官方的Web Clipper插件 甚至可以直接在命令行中 使用defuddle命令来爬取网页内容 那这样更节省token 但是英文资料还是需要 你调用AI来进行翻译 那么以上就是网页 剪藏skill defuddle的使用方法

**3:56** · 接下来是obsidian CLI 那这个skill我在之前有专门 出过一期视频来详细讲解 大家可以回看我的那期视频 那这个skill也是必备的 obsidian CLI是obsidian官方最近 新发布的命令行工具 那目的就是全面拥抱AI智能体 那最近很多厂商都 推出了自家产品的CLI 比如谷歌推出了谷歌全家桶的GWS CLI

### Obsidian CLI skill

**4:22** · 而飞书钉钉也都推出 了自家的CLI工具 那命令行对于智能体来说是 最高效且节省token的方式 那这里我要额外说一个问题 那最近OpenClaw大龙虾非常火 而OpenClaw官方GitHub仓库中的skill 文件夹内就有一个obsidian skill

### 避坑：别用大龙虾自带的过时 Obsidian Skill

**4:38** · 那但是大家要注意 这个skill虽然描述中说 它使用的是obsidian CLI 那实际上它使用的是 一个2023年的开源项目 它并不是官方的CLI工具 而目前这个开源项目的作者 已经把项目改名叫notesmd-cli

**4:55** · 我们可以看到这个仓库的readme中说了 为了和obsidian官方CLI有所区别 所以进行了改名 并且notesmd-cli也不单纯绑定obsidian 那这个工具的本质还是 直接操作Markdown文件 也就是文件系统的I/O操作 那会耗费大量token 而且它也不像obsidian CLI 那样触发obsidian的内部机制 那唯一的优势就是不需要obsidian客户端

**5:20** · 但你很难想象 在你部署OpenClaw宿主机 没有安装obsidian客户端 却有你的obsidian笔记库 那就不管是在Docker还是在云端 都是难以理解的 那所以大家如果使用OpenClaw大龙虾 一定要注意这个skill并 不是一个好的选择 那如果你想要为你的 大龙虾找到合适的skill 推荐去ClawHub这个官方市场 那市场中就有obsidian CLI这个官方skill

**5:45** · 那至于一些第三方的obsidian skill 大家在使用的时候要注意甄别 我们回到obsidian skill仓库 那接下来是obsidian bases这个skill 有关bases数据库 大家可以回看我之前 的视频来进行了解 那这个skill教会了AI如何正确的 编写和修改.base格式的yaml文件 而创建简单的base数据库 一般来说我们只需要 手动创建就可以了 通过智能体调用这个Skill来创建base 通常都是进行知识库架构级别的操作

### Bases Skill：让 AI 生成复杂的数据库视图

**6:16** · 尤其是结合obsidian CLI这个skill 智能体能够清晰地理解你 整个知识库的架构和元数据 然后使用base skill来 生成复杂数据的看板 对特定的笔记进行追踪 那这个skill目前是独一无二 obsidian用户必须安装的 那接下来是obsidian Markdown这个skill 这个skill给智能体制定 了一套标准作业程序 也就是SOP 在智能体撰写obsidian知识笔记的时候 严格按照既定的流程和格式来撰写 那虽然obsidian的专属Markdown语法在AI

### Markdown Skill：如何进行内容扩展

**6:47** · 大模型的训练数据中早就存在了 但是就数据权重和输出概率分布来说 AI会有强烈的向通用标准坍缩的倾向 那更倾向于输出标准的Markdown语法 哪怕在编写obsidian专有语法的时候 也有可能出现幻觉 所以这个skill的价值在于给了 智能体一套标准的流程和约束 我之所以在表格中给 这个skill加了叹号 是我更建议大家对这个skill进行扩展

**7:14** · 因为这个skill是撰写Obsidian 笔记的唯一入口skill 它的职责是格式的约束 所以你可以把你倾向的 格式要求加入到这个skill中 那比如我个人会要求AI 不要使用一级标题 而是从二级标题开始使用 也不要使用嵌套的 无序列表和有序列表 那这些格式要求都是结合我 个人的喜好和实际需求来定的 因为我的知识笔记内容会被用于我 平时发布小红书图文和公众号文章

**7:43** · 那一级标题字号过大 而嵌套列表占用过多空间 非常不美观 所以这一类的格式约束 我都写到了这个skill中 所以我也建议大家把自己的 格式偏好和要求也写入这个skill 那这个skill的内容全部都是Markdown 那不像之前的几个 skill包含大量的代码 哪怕你没有编程基础 依然可以轻松的对内容进行扩展

### JSON-Canvas Skill：为什么不推荐？

**8:06** · 那么最后一个skill是 json-canvas 我几乎每一期视频都会用 canvas白板来展示知识架构 我是非常喜欢canvas的 那这个json-canvas skill同样给 智能体制定了一套标准 那其实json-canvas标准发布的也比较早 那在AI大模型的训练 数据中也是存在的 我早期视频的canvas大多都是用网页AI

**8:28** · 比如GPT Gemini来画的 那效果一般 需要自己手动调整 而obsidian CEO steph发布的这个 skill也存在同样的问题 也就是布局和排版不是很理想 这也是为什么这5个skill里 我唯一不推荐的就是这个skill 更重要的是 我们会有一个更好的skill来替代 那么接下来我就来 介绍这个更好的替代品 Axton的3个画图类skill Axton是著名的AI博主 他的自媒体ID是回到Axton 大家可以关注一下他 他写的canvas skill针对节点的

### 更好的替代品：Axton 重构的三大画图 Skill

### 横向对比：Axton的json canvas skill

**8:59** · 坐标和布局做了非常好的优化 我们直接来看效果 我用同样的提示词生成 了三张canvas思维导图 首先是通过网页版AI让 Gemini3.1 Pro生成的思维导图 那虽然内容详实 但很多节点的大小都没有计算正确 节点中的文字并没有显示全 是需要手动去做调整的 那这也是我以往制作canvas的主要方式 那第二张图是使用obsidian CEO steph写的skill

**9:27** · 那在OpenCode中用免费的大模型生成的 那可以看到思维导图非常的干净整洁 但是细看还是会有一些瑕疵 也就是节点大小计算不准确 文字内容溢出等问题 那第三张图是使用excalidraw的skill生成的

**9:42** · 那所有节点的大小都非常合适 整个图表非常美观 而Axton的skill除了支持思维导图模式 还支持另一种自由图表模式 我来给大家展示一下 那图表中通过canvas的分组来对 知识进行了结构化的展示 也是非常的好用 那即使使用免费大模型 生成的图表样式也非常不错 而今天这期视频所展示的结构图 就是用Axton的这个skill生成的 而Axton还有另外两个画图skill

### Axton的mermaid和excalidraw skill

**10:11** · 分别是mermaid和excalidraw 关于mermaid语法 大家可以回看我之前 发布的mermaid语法教程视频 我直接来为大家展示一些效果 首先是mermaid图表 使用skill生成的mermaid代码 不仅结构清晰 样式美观 那成功率也很高 几乎不需要手动调试 而excalidraw生成的excalidraw 图表也是非常的好看 结构非常清晰完整 非常适合你在做知识梳理 和头脑风暴的时候来使用 excalidraw插件是obsidian插件市场

**10:40** · 中下载量排名第一的插件 所以如果你也喜欢用excalidraw来 做头脑风暴和深度思考 那这个skill是你最好的选择 那么以上就是Axton的 三个画图相关的skill 这三个skill基本覆盖了你在 obsidian中所有的图表制作场景 那最后两个skill是深度知识学习的skill 一个是Tutor skill 一个是scholar skill 那这两个skill非常有价值 那它们的共同点是都能够 生成一整个完整的obsidian知识库 包含双链MOC内容地图和结构化标签

### 深度学习利器：Tutor 与 Scholar 工作流对比

**11:14** · 那我之前发布过一期视频 有关如何使用obsidian来 学习一个陌生的知识领域 而现在仅仅过去了半年 我的整套工作流就已经 能够被skill完全替代了 而这两个skill的区别 就像这两个skill的名字 tutor skill更适合快速的 学习一些知识文档和资料 非常适合应付考试和面试 另外它还支持代码项目 所以如果你手上有一些 需要快速学习的文档资料 或者想要快速学习一个GitHub项目 那这个skill是非常合适的

**11:44** · 所以这个skill是我着重推荐的skill 它适合绝大多数场景 而scholar skill则是更适合学术科研 是专门针对论文学习的skill 而这个skill是一个重度 的工作流编排系统 具备记忆管理能力和长周期任务编排

**12:00** · 所以这个skill深度依赖OpenClaw大龙虾 那它有三级阅读体系 其中L3级别的深度阅读是一个 长生命周期的异步编排任务 会耗费几个小时 有一整套科研模拟循环 那当然呢 它非常的消耗token 那所以这个skill不管是 底层机制还是功能上 都更适合Openclaw大龙虾 那再次强调 它非常的消耗token 那么我接下来就来展示Tutor skill 而scholar skill就留给对学术

### Tutor 实操演示：PDF 全自动转成双链知识库

**12:28** · 科研有需要的同学来尝试 我使用的智能体是Opencode 安装了Tutor skill之后 新建一个空白文件夹 在文件夹里创建两个文件夹 一个叫resources 一个叫StudyVault 记住这个名字是skill的硬性规定 skill会在StudyVault里创建 一个完整的obsidian知识库 在resources文件夹中 我放入一个PDF 这个是谷歌发布的提示词工程的教程

**12:55** · 然后在OpenCode中使用 当前目录作为工程目录 然后在AI对话框中输入 /tutor-setup 然后回车 它自动就生成了一大段提示词 然后智能体就开始工作了 整个过程会比较漫长 这里我就对视频进行一个加速 那使用Opencode是因为它 有很多的免费AI额度 像小米和千问的最新的大模型

**13:20** · 都可以在这里免费使用那另外 我目前展示的案例中只有一个PDF 如果你手上有一堆PDF 那么智能体消耗的 token自然也就会增加 那另外你也可以利用这个skill来 分析GitHub上的开源项目 而这个skill是支持分析代码仓库的 那这个功能是让我最喜欢的 那我们可以看到智能体 已经完成了知识库的搭建 我们打开obsidian 用obsidian打开StudyVault这个文件夹

**13:46** · 可以看到智能体已经根据PDF 资料中的知识内容创建了一 整个精美的obsidian笔记库 并撰写了知识笔记 还对笔记进行了双向链接的关联 那点开知识图谱就能 看到笔记之间的关联了 那点开文件夹就能看到对应的 知识笔记和MOC内容地图 那可以看到知识笔记 的内容是非常精美的 尤其是MOC内容地图 那非常的不错 另外skill还创建了针对 考试的专项训练笔记 特别适合你准备考试或者面试

**14:17** · 而笔记中还会用代码块来 绘制一些概念的结构图 虽然笔记中还是有一些小瑕疵 但总体问题不大 我们回到OpenCode 输入/tutor 就可以生成测试题 能够非常好的检验你的学习成果 这里我们就大概的看一下 那测试题做完之后 它还会生成测试结果以及学习仪表板

**14:40** · 可以看到在知识图谱中出现 了一个学习仪表板的区域 那点开仪表板就能看到你对 每个知识领域的掌握程度 那这个skill是非常适合你 做一些针对性的学习的 那么另一个skill也就是scholar skill 我就不多做展示了 那如果你有学术科研需求 可以在OpenClaw大龙虾里进行尝试 那scholar skill有不少的skill依赖 其中有关obsidian的依赖 是obsidian-direct这个skill 这个skill的本质依然是文件的读取

### Scholar 工作流：适合学术科研，但极耗 Token

**15:10** · 也就是I/O操作 那非常的消耗token 所以建议大家在日常的学习过程中 优先使用tutor skill 只有当你需要做深度 的学术科研的时候 再使用scholar skill 最后我们来快速看一下两个obsidian

### 智能体插件配置教程一：Claudian

**15:26** · 智能体插件的安装和配置方法 首先是claudian 也就是obsidian集成Claude code的插件 那在我之前的视频中已经 深入地为大家讲解过了 那么我们来快速过一遍步骤 首先你要安装好Claude code 然后我们需要在第三方 插件市场中安装BRAT插件 在BRAT插件的设置中点击add Beta plugin

**15:47** · 输入仓库地址 然后点击添加 这个插件就安好了 那当然你也可以手动下载 Claudian插件的三个核心文件 并放到obsidian的plugins文件夹里 我们来到claudian的插件设置界面 那在个性化设置这里填写一个称呼 比如填写Jason 然后拉到最下面 环境变量这里 如果你所在的地区是被 Anthropic所限制的地区 你就需要填写这几个 变量来转接兼容模型 那具体的设置我已经展示在屏幕上了

**16:18** · 那另一个插件obsidian-agent-client 那这个插件更强大一些 支持大部分主流智能体工具 比如Claude code codex Gemini CLI OpenCode 那甚至是阿里的千问code 同样使用BRAT安装 安装之后来到设置界面 以OpenCode为例 我们在custom agent中点击add按钮 添加一个智能体 设置agent ID和display name为OpenCode

### 智能体插件配置教程二：Obsidian-agent-client

**16:44** · 然后把OpenCode的路径填入path 那如果你不知道OpenCode安装在哪 那么打开命令行 输入where opencode就能找到 那另一个arguments这个参数 填入我屏幕上的信息 那注意第三行的路径是你 的obsidian知识库的路径 那设置完这些之后 重启obsidian 然后打开插件的AI对话框 那输入你好 如果AI回复了 那就配置成功了 那这两个插件是Obsidian目前 最主流的智能体插件 两个插件的图标都是机器人图标

**17:17** · 大家根据自己的需求 选择一个就可以了 那么今天的视频到这里就结束了 我留下一个开放性的问题 刚才智能体给我们创建的obsidian仓库 中有非常精美的MOC内容地图笔记 那么我的问题是 在MOC里到底是应该手动

### 总结与开放性思考：MOC的设计

**17:34** · 编写关联笔记并设置双链 还是应该使用base数据库呢 如果使用base数据库 它是不会产生双链的 那么MOC的意义何在呢 这个开放性的问题留给大家进行思考 那欢迎大家在评论区写出你的想法 那如果你在使用这些skill的过程中 有任何的问题 也欢迎留言和我讨论 今天视频中的知识笔记 可以在我的个人主页中下载 你可以在我的频道信息中 找到我的个人主页地址 那记得点赞关注 谢谢大家