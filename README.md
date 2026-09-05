# concept-study

> "概念学习资料生成 Skill" 的实践仓库 —— 用 AI 把任意概念讲清楚，并沉淀为可复用的工作流。

## 仓库结构

```
concept-study/
├── README.md                                 # 本文件
├── .workbuddy/
│   └── skills/
│       └── concept-study/                    # 项目级 Skill
│           └── SKILL.md                      # 工作流定义
└── docs/
    └── concepts/                             # 学习资料输出目录
        ├── agent.md                          # 概念：Agent
        ├── context.md                        # 概念：大模型的上下文
        └── skill.md                          # 概念：Skill
```

## 这套 Skill 做什么

它被设计来响应"学习 X / 讲解 X / 介绍 X / 什么是 X"等学习类请求。命中触发词后，AI 会：

1. 自动加载 `SKILL.md` 里的工作流
2. 选定合适的可视化类型（流程图 / 结构图 / 示意图 / 图表）
3. 生成一张内联 widget + 400-700 字的讲解
4. 把成品落盘到 `docs/concepts/<slug>.md`

## 已生成的学习资料

| 概念 | 文件 | 配套图示类型 |
|------|------|--------------|
| Agent | [docs/concepts/agent.md](docs/concepts/agent.md) | 循环流程图 |
| 大模型的上下文 | [docs/concepts/context.md](docs/concepts/context.md) | 嵌套结构图 |
| Skill | [docs/concepts/skill.md](docs/concepts/skill.md) | 目录结构图 |

## 怎么使用这个 Skill

**在 WorkBuddy 里**：打开本仓库（或用 WorkBuddy 打开这个项目），当你说"学习 XXX"时，AI 会自动识别并调用本 Skill。

**改造它**：直接编辑 `.workbuddy/skills/concept-study/SKILL.md`，改触发词、工作流、风格条款即可。所有改动下次对话立即生效。

**在团队里复用**：其他人 `git clone` 后，会自动获得这个 Skill（因为是项目级的，绑定在仓库里）。

## 设计原则

- **图示先行**：每个概念配一张图，先有视觉直觉再讲细节。
- **讲解给初学者**：假设读者已知基础术语，但不熟悉本概念。
- **风格统一**：所有图用 SVG 内嵌，遵循扁平、无渐变、无 emoji 的设计 token。
- **可被复制**：Skill 本身是文档化的，别人读 `SKILL.md` 就能复制这套工作流到自己的项目。

## 后续扩展方向

- 在 `references/` 目录加入领域知识（如设计原则、术语表）
- 增加 `scripts/` 自动生成索引页 `docs/README.md`
- 拆出子 Skill：`concept-study-flowchart`、`concept-study-chart` 等

---

_生成于 2026-09-05，由 WorkBuddy 协助创建。_