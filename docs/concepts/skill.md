# Skill

Skill 是 WorkBuddy（以及 Anthropic、Claude Code 等同类 AI 编程助手）里的**可复用"专业能力包"**——它把某个领域的"该怎么做、不能怎么做、参考什么资料"打包成一份声明式文档，让 AI 在合适的时候自动调用。

## 图示

<svg viewBox="0 0 680 380" width="100%" role="img">
  <title>Skill 文件结构</title>
  <desc>一个 Skill 是一个文件夹，核心是 SKILL.md（含 YAML 前言描述元数据），可以附带脚本、参考、模板等资产</desc>
  <text x="340" y="30" class="th" text-anchor="middle" dominant-baseline="central">Skill 的目录结构</text>
  <g><rect x="40" y="60" width="600" height="290" rx="20" fill="#F1EFE8" stroke="#5F5E5A" stroke-width="0.5"/><text x="60" y="85" class="th" fill="#2C2C2A">.workbuddy/skills/concept-study/</text><text x="60" y="103" class="ts" fill="#5F5E5A">一个 Skill = 一个文件夹</text></g>
  <g><rect x="60" y="125" width="560" height="70" rx="10" fill="#EEEDFE" stroke="#534AB7" stroke-width="0.5"/><text x="80" y="148" class="th" fill="#3C3489">SKILL.md</text><text x="80" y="166" class="ts" fill="#534AB7">YAML 前言（name/description/触发词）</text><text x="80" y="184" class="ts" fill="#534AB7">+ Markdown 正文（工作流、风格、边界）</text></g>
  <g><rect x="60" y="210" width="180" height="60" rx="10" fill="#E1F5EE" stroke="#0F6E56" stroke-width="0.5"/><text x="80" y="232" class="th" fill="#085041">scripts/</text><text x="80" y="250" class="ts" fill="#0F6E56">可执行脚本</text></g>
  <g><rect x="250" y="210" width="180" height="60" rx="10" fill="#FAEEDA" stroke="#854F0B" stroke-width="0.5"/><text x="270" y="232" class="th" fill="#633806">references/</text><text x="270" y="250" class="ts" fill="#854F0B">领域参考文档</text></g>
  <g><rect x="440" y="210" width="180" height="60" rx="10" fill="#FBEAF0" stroke="#993556" stroke-width="0.5"/><text x="460" y="232" class="th" fill="#72243E">assets/</text><text x="460" y="250" class="ts" fill="#993556">模板 / 图示 / 示例</text></g>
  <g><rect x="60" y="290" width="560" height="40" rx="10" fill="#E6F1FB" stroke="#185FA5" stroke-width="0.5"/><text x="80" y="314" class="th" fill="#0C447C">触发时机：用户意图命中 triggers 列表 → 自动加载 → 工作流生效</text></g>
</svg>

## 讲解

很多人第一次听说 Skill，会以为它是"插件"或"宏"——其实 Skill 更像是**写给 AI 的岗位手册**。一份好的 Skill 告诉 AI：在什么场景下你要上场、上场后按什么步骤操作、可以参考哪些资料、哪些事不要做。这样 AI 在被问到相关问题时，就会自动"切角色"到那个领域，按照手册行事。

一份 Skill 的物理形态就是一个**文件夹**，里面最关键的文件叫 `SKILL.md`。它由两部分组成：**YAML 前言**（描述这个 Skill 的元数据：叫什么、描述什么、在哪些用户意图下被触发）和 **Markdown 正文**（具体的工作流、风格指南、边界条件）。AI 读取 Skill 时是先看前言判断"是不是该用我"，再看正文理解"该怎么用"。

除了 `SKILL.md`，文件夹里还可以放三类资产让 Skill 更实用：`scripts/` 放可执行脚本（让 AI 直接调用）、`references/` 放领域参考文档（让 AI 读取背景知识）、`assets/` 放模板和示例（让 AI 模仿输出格式）。Skill 越复杂、越专业，这些附件就越重要——比如做金融分析的 Skill 可能带一个计算估值的 Python 脚本，做排版的 Skill 可能带字体模板。

Skill 有两种"作用域"。**用户级**（`~/.workbuddy/skills/`）所有项目都能用，相当于 AI 的"个人工具箱"；**项目级**（`<repo>/.workbuddy/skills/`）只在该仓库生效，相当于"团队共享的 SOP"。这次我们创建的就是项目级 Skill，方便团队成员克隆仓库后自动获得"概念学习"能力。

Skill 和"系统提示词"（system prompt）有本质区别。系统提示词是**每次对话都强制加载**的通用人设（比如"你是 WorkBuddy"），而 Skill 是**按需触发**的专业能力，只在匹配触发词时加载到上下文。两者配合，AI 既保持稳定人格、又能在专业场景里变身专家。

易混淆点：**Skill 不是单纯的"指令集"**，更像是一种"可编程的人格扩展"。它不写代码、也不被 AI 直接执行，而是通过描述意图和工作流，让 AI 自己"读懂后照着做"。所以写好 Skill 的关键不是写得多详细，而是写得**足够清晰、足够具体**——能像把工作交接给新同事那样，把判断依据、典型场景、反例都讲清楚。