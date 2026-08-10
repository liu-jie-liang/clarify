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

**研究阶段**：clarify 读你提到的 GATE_RULES.md，发现它只定义了教学流程规则但没涉及工具调用权限。追读 `.trae-cn/` 下的配置文件，找到权限相关项。

**clarify 会追问：**

| 问 | 你答 |
|---|------|
| "你说的不确认是指：AI 在问答中顺手给出修改方案，还是 AI 真的调用了 Edit 工具写入文件？" | 真的调了 Edit |
| "是某个特定会话这样，还是所有会话都这样？" | 所有 |
| "你想查的是 Trae IDE 自己有没有权限控制，还是所有 AI 编程助手的通用做法？" | 都想要 |

**最终生成的文本（完整 brief）：**

````
注：涉及文档或 skill 编辑的任务，自动遵循 user_profile.md 中的注意力原则。

- 背景：
  项目已做 skill 和文档体系注意力重构，GATE_RULES.md 中定义了禁止行为规则（禁止不经确认修改文件），
  但文本规则无法阻止 AI 的 Edit/Write 工具调用。当前 .trae-cn/ 下无工具调用权限配置文件。

- 本轮发现的问题：
  问题一：GATE_RULES.md 禁止行为只约束了"什么不该做"，但 AI 的工具调用不经过规则文本判断
  （证据：GATE_RULES.md#禁止行为，无工具调用拦截机制）
  问题二：.trae-cn/ 目录下无权限模式配置文件
  （证据：~/.trae-cn/ 仅含 memory/ 和 github-credentials.md）

- 需读的文件：
  /Users/liujieliang/.trae-cn/ — 权限相关配置
  /Users/liujieliang/Documents/trae_projects/trae/read_code/config/GATE_RULES.md
  .cursor/rules/ 、 .windsurfrules 等业内工具的权限模式文件（搜索后用 WebFetch 读取官方文档）

- 具体修改：
  1. 读 ~/.trae-cn/ 目录全部配置文件，确认是否有权限模式开关
  2. 搜索 Cursor、Windsurf、Claude Code 的系统级权限模式实现方式
  3. 对比 Trae IDE 的机制，给出是否可通过系统配置（非 prompt）限制工具调用的结论

- 为什么：从 prompt 约束升级为系统级权限控制，解决"AI 跨越确认边界"的根本问题。
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

**研究阶段**：clarify 读 `day09_teaching.py` 了解 delta.content 的上下文，读 `verify/` 下的现有验证脚本，读 `GATE_RULES.md` 和 `RULES.md` 中的验证规则。

**clarify 追问后生成的文本（完整 brief）：**

```
注：涉及文档或 skill 编辑的任务，自动遵循 user_profile.md 中的注意力原则。

- 背景：
  项目为 AI 辅助教学项目，day09_teaching.py 第 62-140 行讲解 stream=True 时 delta.content 的访问方式。
  GATE_RULES.md 禁止行为 #2 规定"禁止跨端点推断"——不能用一个端点的行为推断另一个。
  上一轮会话中 AI 只查了 OpenAI 文档就推断 Ollama 和 DeepSeek 的行为，并直接修改了代码，
  违反上述规则。会话已污染，需要在新会话中继续。

- 本轮发现的问题：
  问题一：day09_teaching.py 中有多处 `delta.content` 直接访问，但从未实验验证过
  （证据：day09_teaching.py#L62-L140，所有 delta.content 访问均为直接访问，无 try-except 保护）
  问题二：现有验证脚本覆盖不全——verify_delta_attrs.py 和 verify_content_attr.py 只测了 DeepSeek
  （证据：verify_delta_attrs.py#L1-L176，仅含 DeepSeek API 调用）
  问题三：GATE_RULES.md 禁止跨端点推断，但 AI 仍做了推断
  （证据：GATE_RULES.md#禁止行为）

- 需读的文件：
  /Users/liujieliang/Documents/trae_projects/trae/read_code/llm-chat/day09_teaching.py
  /Users/liujieliang/Documents/trae_projects/trae/read_code/llm-chat/verify/verify_delta_content.py
  /Users/liujieliang/Documents/trae_projects/trae/read_code/config/GATE_RULES.md
  /Users/liujieliang/Documents/trae_projects/trae/read_code/config/RULES.md

- 具体修改：
  1. 先别改 day09_teaching.py 的代码
  2. 对 DeepSeek 端点写 verify 脚本跑实验，确认 delta.content 是否在所有 chunk 中始终存在
  3. 对 Ollama 端点写 verify 脚本跑实验，确认 delta.content 是否在所有 chunk 中始终存在
  4. 两个实验都跑完后再根据结果决定是否需要给 delta.content 加保护性访问
  注意：用 Python OpenAI 库分别操作两个端点，验证脚本放 llm-chat/verify/ 目录下。

- 为什么：用实验证据替代文档推断，补全双端点验证覆盖，让 day09_teaching.py 的断言有据可查。
```

---

## 和 ask-coach 的区别

| | clarify | ask-coach |
|---|---|---|
| 适用场景 | 任何新问题 | 仅 learning-coach 教学会话的沟通事故 |
| 绑规则 | 否 | 是（读 RULES.md + GATE_RULES.md） |
| 输入 | 你的模糊想法 | 你和 AI 的沟通事故 |

如果是教学会话遇到问题 → 用 ask-coach。遇到任何其他新问题说不清楚 → 用 clarify。
