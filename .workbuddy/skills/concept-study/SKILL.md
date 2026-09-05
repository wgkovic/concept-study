---
name: concept-study
description: 当用户输入包含"学习 X"、"讲解 X"、"介绍 X"、"了解 X"、"科普 X"、"什么是 X"等学习意图时，调用本 Skill 生成一个概念的学习资料。输出为 1 张可视化图示（SVG/HTML widget）+ 400-700 字的 Markdown 讲解，存为 docs/concepts/<slug>.md。
triggers:
  - 学习 X
  - 讲解 X
  - 介绍 X
  - 了解 X
  - 科普 X
  - 什么是 X
  - teach me X
  - explain X
inputs:
  - name: concept
    required: true
    description: 用户要学习的概念名（中文或英文皆可）
  - name: depth
    required: false
    default: standard
    options: [brief, standard, deep]
outputs:
  - path: docs/concepts/<slug>.md
    description: 概念学习资料，包含 1 个 widget 块和 400-700 字讲解
---

# concept-study

为任意概念生成一份图文并茂的学习资料。

## 工作流

1. **解析概念**：从用户输入里提取 `concept`，把中文/英文/缩写都保留（一个 slug 用作文件名）。
2. **选定图示类型**（按概念语义）：
   - 流程/步骤 → flowchart（SVG，最多 4-5 节点）
   - 结构/层级 → structural（嵌套容器 SVG）
   - 直觉/机理 → illustrative（带空间隐喻的 SVG）或 HTML stepper
   - 数据/对比 → Chart.js 图
   - **复杂主题** → 多个 widget 串讲，每图配一段散文
3. **写讲解**：400-700 字，按"是什么 → 为什么 → 怎么工作 / 由什么组成 → 易混淆点"组织。用 Markdown 二级标题分节，避免一行一段。
4. **输出文件**：写入 `docs/concepts/<slug>.md`。文档结构：
   ```markdown
   # <概念>
   <一句定义>

   ## 图示
   <widget SVG/HTML>

   ## 讲解
   ...
   ```
5. **回复用户**：用一句提示 + 告知文件路径 + 文件链接（在 WorkBuddy 中可直接预览）。

## 风格

- 讲解给一个"刚开始接触这个概念"的人看，假设读者已知最基本的术语。
- 避免堆术语。每个术语首次出现给一句白话解释。
- 配图有标签（中文 sentence case，≤5 字/词）。

## 边界

- 不生成金融/医疗/法律建议。
- 不输出超过 800 字的讲解——若概念过大，拆成多个子概念各学一次。
- 不在 SVG 里放 emoji、渐变、阴影。

## 示例

> 用户：学习 Agent
> 输出：docs/concepts/agent.md + 调用 show_widget 展示 Agent 循环示意图