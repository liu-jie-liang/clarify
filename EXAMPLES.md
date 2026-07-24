# EXAMPLES — clarify 示例

> L2 按需加载。

---

## 示例 1：AI 不确认就改文件

**你脑子里想的：**

> AI 老是跳过确认直接改代码，我加了 GATE_RULES.md，它还是不遵守。我也不想每次都说"别改先确认"，一纠正会话就膨胀。有没有办法从系统层面拦住？

**在新会话输入：**

````
Use Skill: clarify

AI 在回答问题时经常不经过我确认就直接修改文件。我重构了 skill
和文档体系（加了禁止行为规则），但 AI 仍然会越过"先确认再动手"的边界。
我想知道业内是怎么从系统层面解决这个问题的。
````

**clarify 会追问：**

| 问 | 你答 |
|---|------|
| "你说的不确认是指：AI 在问答中顺手给出修改方案，还是 AI 真的调用了 Edit 工具写入文件？" | 真的调了 Edit |
| "是某个特定会话这样，还是所有会话都这样？" | 所有 |
| "你想查的是 Trae IDE 自己有没有权限控制，还是所有 AI 编程助手的通用做法？" | 都想要 |

**最终生成的文本：**

````
我的 Trae IDE 中，AI 在所有会话里都会在问答过程中不经确认就调用
Edit 工具直接修改文件。我已经在项目里加了 GATE_RULES.md 禁止行为
规则，但文本规则无法阻止 AI 的工具调用。

需要你帮我做两件事：

1. 读 /Users/liujieliang/.trae-cn/ 目录下的配置文件，找权限相关设置
2. 搜一下 Cursor、Windsurf、Claude Code 是如何通过系统级
   权限模式（非 prompt 约束）限制 AI 在回答问题时改文件的

基于发现，告诉我 Trae IDE 有没有等效机制。
````

---

## 示例 2：对 AI 说了三遍"验证"它还在改代码

**你脑子里想的：**

> 我让 AI 验证 delta.content 在 DeepSeek 和 Ollama 上的行为，它就拿 OpenAI 文档推了个结论，然后直接改代码。我说"你应该验证一下"，它说"好"然后继续改。说了三遍都没用，会话全被纠缠污染了。开新会话的话我又不知道怎么描述这个烂摊子。

**在新会话输入：**

```
Use Skill: clarify

之前会话里，AI 查了 OpenAI 文档就说 delta.content 的行为是 XXX，
然后直接改了代码。我让它先做实验验证 Ollama 和 DeepSeek 端点，
说了三次它都不听，会话已经被污染了。开新会话怎么继续？
```

**clarify 追问后生成的文本：**

```
根据之前的教学，day09_teaching.py 中有多处 `delta.content` 直接访问。
AI 在上一轮只查了 OpenAI 文档就推断 Ollama 和 DeepSeek 的行为，
我得出的结论是：

1. delta.content 在 Ollama 端点是否一定存在——需要实验验证
2. delta.content 在 DeepSeek 端点是否一定存在——需要实验验证

先别改代码。对两个端点分别写 verify 脚本跑实验，
确认行为后再决定是否需要改 delta.content 的访问方式。
```

---

## 和 ask-coach 的区别

| | clarify | ask-coach |
|---|---|---|
| 适用场景 | 任何新问题 | 仅 learning-coach 教学会话的沟通事故 |
| 绑规则 | 否 | 是（读 RULES.md + GATE_RULES.md） |
| 输入 | 你的模糊想法 | 你和 AI 的沟通事故 |

如果是教学会话遇到问题 → 用 ask-coach。遇到任何其他新问题说不清楚 → 用 clarify。
