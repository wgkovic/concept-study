# 大模型的上下文

"上下文"（context）是大语言模型**每一次生成回复时能"看得到"的全部输入文本**，它决定了模型知道什么、忘掉什么。

## 图示

<svg viewBox="0 0 680 420" width="100%" role="img">
  <title>LLM 上下文结构</title>
  <desc>大模型上下文窗口由系统提示、用户消息、助手消息、工具结果、推理等组成，总长度受 token 上限约束</desc>
  <text x="340" y="30" class="th" text-anchor="middle" dominant-baseline="central">LLM 上下文窗口</text>
  <text x="340" y="52" class="ts" text-anchor="middle" dominant-baseline="central" fill="#5F5E5A">所有模型看得到的输入总和，上限 = context window</text>
  <g><rect x="40" y="80" width="600" height="300" rx="20" fill="#EEEDFE" stroke="#534AB7" stroke-width="0.5"/><text x="60" y="105" class="th" fill="#3C3489">context window</text><text x="60" y="123" class="ts" fill="#534AB7">比如 128k tokens 约 9.6 万汉字</text></g>
  <g><rect x="60" y="140" width="560" height="50" rx="8" fill="#E1F5EE" stroke="#0F6E56" stroke-width="0.5"/><text x="80" y="160" class="th" fill="#085041">system prompt</text><text x="80" y="178" class="ts" fill="#0F6E56">角色设定、规则约束、工具说明</text><text x="540" y="165" class="th" text-anchor="end" fill="#085041">~5%</text></g>
  <g><rect x="60" y="200" width="270" height="100" rx="8" fill="#E6F1FB" stroke="#185FA5" stroke-width="0.5"/><text x="80" y="220" class="th" fill="#0C447C">user / 历史对话</text><text x="80" y="238" class="ts" fill="#185FA5">用户消息 + 助手回复</text><text x="80" y="256" class="ts" fill="#185FA5">（多轮累积）</text><text x="310" y="252" class="th" text-anchor="end" fill="#0C447C">~30%</text></g>
  <g><rect x="340" y="200" width="280" height="100" rx="8" fill="#FAEEDA" stroke="#854F0B" stroke-width="0.5"/><text x="360" y="220" class="th" fill="#633806">工具结果 / 检索片段</text><text x="360" y="238" class="ts" fill="#854F0B">RAG 召回、API 返回</text><text x="360" y="256" class="ts" fill="#854F0B">代码执行输出</text><text x="600" y="252" class="th" text-anchor="end" fill="#633806">~40%</text></g>
  <g><rect x="60" y="310" width="560" height="50" rx="8" fill="#FBEAF0" stroke="#993556" stroke-width="0.5"/><text x="80" y="330" class="th" fill="#72243E">思维链 / scratchpad</text><text x="80" y="348" class="ts" fill="#993556">模型内部推理、计划、todo 列表</text><text x="540" y="335" class="th" text-anchor="end" fill="#72243E">~25%</text></g>
  <text x="340" y="405" class="ts" text-anchor="middle" dominant-baseline="central" fill="#5F5E5A">溢出部分会被截断或压缩 → 丢失早期信息</text>
</svg>

## 讲解

很多人把 LLM 当成一个"有记忆"的聊天伙伴，但其实**模型本身没有任何长期记忆**——它每次生成下一个字时，看的只是当前轮喂给它的全部文本，这个文本的总和就叫上下文。所以同一个模型，在新对话开始时不会记得上一段对话的任何内容（除非显式把那段历史粘回去）。

"上下文窗口"（context window）是模型的硬性容量上限，常用单位是 **token**——大约一个汉字 1-2 个 token，一个英文单词 1-2 个 token。当代主流模型的上限在 32k-200k tokens 之间，相当于几万到十几万汉字。**一旦总输入超过这个上限，多出来的内容会被截断或者压缩，模型就"看不见"了**。这就是为什么长文档总结、超长代码库分析这类任务需要专门的策略（RAG、分块、摘要压缩）。

上下文窗口里装的东西五花八门，典型包括：**系统提示**（system prompt，告诉模型"你是谁、能做什么、规则是什么"）、**用户消息和助手历史**（多轮对话累积）、**工具结果**（Agent 调用 API 或 RAG 检索回来的片段）、**思维链**（让模型"先想再答"的推理痕迹）。不同厂商对这些槽位的命名和拼接顺序略有差异，但本质都是把这些文本拼成一个大字符串喂给模型。

理解上下文最关键的一点是：**模型表现好不好，往往不取决于它"聪不聪明"，而取决于上下文设计得好不好**。同样的 GPT-4，给它一份清晰结构化的指令和参考资料，效果会远超一段模糊口语化的请求。提示工程（prompt engineering）本质就是在有限上下文里最大化有效信息密度。

易混淆点：**上下文 ≠ 知识**。模型训练阶段学到的世界知识存在参数里，而上下文是运行时塞进去的临时信息。把一个事实写进系统提示，模型"看得到"但不一定"相信"；经过训练学到的知识，模型默认就会用。两者协作，才让模型既博学又随任务可定制。