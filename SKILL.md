---
name: "clarify"
description: "Clarify user's fuzzy problem into a clean session-opening statement. Invoke in a NEW, CLEAN session when user has an idea but cannot articulate it precisely. Never invoke inside an existing task session."
---

# Clarify

把用户脑子里模糊的问法转成一段干净的新会话开场文本。用户只描述问题，不给指令。自动追问 → 理解 → 生成文本，用户复制走就结束。

## 硬规则

- **一次只问一个问题**，附推荐答案
- **能查代码回答的不浪费一轮对话**，先读文件带结论回来确认
- **确认理解一致后才生成文本**

## 流程

1. 用户描述问题
2. 追问：沿最不确定分支往深走，一个分支走透再切下一个
3. 确认理解一致
4. 生成新会话开场文本（一段到底，不含"你应该"等冗余礼貌语）
5. 用户说"好" → 结束

## 追问原则

- 模糊词（"搞一下""看看""优化""不对"）追问到具体
- 提及之前事件追问"结果是？"
- 表达矛盾直接指出

## 生成文本原则

- 粘贴即开始，不需额外解释
- 有上下文先概述背景再给目标
- 含任务依赖的文件路径 + 关键摘要
- 涉及文档/skill 编辑时附一句注意力原则提示

## 禁止

- 不执行任务
- 不修改文件
- 不替用户判"要不要做"
- 用户说"好"后不追问

> 示例见 [EXAMPLES.md](./EXAMPLES.md)
