# Agent

Agent（智能体）是能够**感知环境、自主决策、采取行动并对结果负责**的 AI 程序。普通 AI 是一次性"回答问题"，Agent 是"为完成一个目标连续做很多事"。

## 图示

<svg viewBox="0 0 680 320" width="100%" role="img">
  <title>Agent 工作循环</title>
  <desc>Agent 通过感知、思考、行动、观察、反思五个步骤与外部环境交互的循环流程</desc>
  <defs>
    <marker id="arrow" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse">
      <path d="M2 1L8 5L2 9" fill="none" stroke="context-stroke" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
    </marker>
  </defs>
  <text x="340" y="30" class="th" text-anchor="middle" dominant-baseline="central">Agent 工作循环</text>
  <text x="340" y="52" class="ts" text-anchor="middle" dominant-baseline="central" fill="#5F5E5A">感知 → 思考 → 行动 → 观察 → 反思</text>
  <g><rect x="40" y="120" width="120" height="60" rx="10" fill="#E6F1FB" stroke="#185FA5" stroke-width="0.5"/><text x="100" y="144" class="th" text-anchor="middle" dominant-baseline="central" fill="#0C447C">感知</text><text x="100" y="164" class="ts" text-anchor="middle" dominant-baseline="central" fill="#185FA5">读环境/任务</text></g>
  <g><rect x="180" y="120" width="120" height="60" rx="10" fill="#EEEDFE" stroke="#534AB7" stroke-width="0.5"/><text x="240" y="144" class="th" text-anchor="middle" dominant-baseline="central" fill="#3C3489">思考</text><text x="240" y="164" class="ts" text-anchor="middle" dominant-baseline="central" fill="#534AB7">推理 + 规划</text></g>
  <g><rect x="320" y="120" width="120" height="60" rx="10" fill="#E1F5EE" stroke="#0F6E56" stroke-width="0.5"/><text x="380" y="144" class="th" text-anchor="middle" dominant-baseline="central" fill="#085041">行动</text><text x="380" y="164" class="ts" text-anchor="middle" dominant-baseline="central" fill="#0F6E56">调用工具/输出</text></g>
  <g><rect x="460" y="120" width="120" height="60" rx="10" fill="#FAEEDA" stroke="#854F0B" stroke-width="0.5"/><text x="520" y="144" class="th" text-anchor="middle" dominant-baseline="central" fill="#633806">观察</text><text x="520" y="164" class="ts" text-anchor="middle" dominant-baseline="central" fill="#854F0B">看执行结果</text></g>
  <line x1="160" y1="150" x2="180" y2="150" stroke="#5F5E5A" stroke-width="1.5" marker-end="url(#arrow)"/>
  <line x1="300" y1="150" x2="320" y2="150" stroke="#5F5E5A" stroke-width="1.5" marker-end="url(#arrow)"/>
  <line x1="440" y1="150" x2="460" y2="150" stroke="#5F5E5A" stroke-width="1.5" marker-end="url(#arrow)"/>
  <path d="M 580 120 Q 640 150 580 180" fill="none" stroke="#5F5E5A" stroke-width="1.5" marker-end="url(#arrow)"/>
  <text x="630" y="155" class="ts" text-anchor="middle" dominant-baseline="central" fill="#5F5E5A">反馈</text>
  <g><rect x="40" y="240" width="540" height="50" rx="10" fill="#FBEAF0" stroke="#993556" stroke-width="0.5"/><text x="100" y="262" class="th" text-anchor="middle" dominant-baseline="central" fill="#72243E">反思</text><text x="100" y="278" class="ts" text-anchor="middle" dominant-baseline="central" fill="#993556">评估是否需要再循环</text><line x1="200" y1="265" x2="540" y2="265" stroke="#993556" stroke-width="0.5" stroke-dasharray="3,3"/><text x="370" y="262" class="ts" text-anchor="middle" dominant-baseline="central" fill="#72243E">成功 → 结束</text><text x="370" y="278" class="ts" text-anchor="middle" dominant-baseline="central" fill="#72243E">未达目标 → 回到感知再次尝试</text></g>
  <line x1="100" y1="180" x2="100" y2="240" stroke="#993556" stroke-width="1.5" marker-end="url(#arrow)" stroke-dasharray="4,3"/>
  <text x="115" y="215" class="ts" dominant-baseline="central" fill="#993556">回到感知</text>
</svg>

## 讲解

Agent 和普通聊天机器人的最大差别在于**它会主动使用工具、并能从工具结果中继续推理**。当你问聊天机器人"今天北京天气怎么样"，它通常只能凭训练数据回答；Agent 则会调用天气 API、读返回结果、然后再用自然语言告诉你——整个过程可能横跨多次模型调用。

Agent 的核心工作流可以拆成五步：**感知**（读懂任务和当前状态）、**思考**（决定下一步做什么、要不要调工具）、**行动**（执行工具调用或生成回复）、**观察**（拿到执行结果）、**反思**（判断目标是否达成，没达成就再走一轮）。这一步环状的循环是 Agent 与一次性 LLM 调用的关键区别——它让模型有机会"自我纠错"。

工程上 Agent 通常由三部分组成：**LLM 大脑**（负责推理和决策）、**工具集**（可以调用的函数/API，比如搜索、读写文件、执行代码）、**记忆/状态**（保留之前步骤的输入输出）。这三者配合，Agent 才能完成跨多步的复杂任务，比如"帮我订明天早上从北京到上海最便宜的高铁票并加入日历"——这至少要 5-8 轮工具调用。

要注意的是，**Agent 不是越大越好**。能力越强的模型不一定更适合做 Agent：推理稳定、能严格遵循格式（如 JSON 输出）、对工具调用 schema 鲁棒的模型才更适合。要区分"通用 LLM"和"Agent 用 LLM"，后者对工具调用微调得更多。

易混淆点：Agent ≠ 多轮对话。多轮对话只是用户和模型反复说话；Agent 强调**自主选择行动**——即使没人追问，它也会自己继续推进任务，直到目标完成或达到步数上限。