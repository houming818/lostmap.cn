---
title: "[SPR-074] 为什么 Loss 在下降，模型却只会一句话：C10 勘误与证据撤回"
date: 2026-07-28
lastmod: 2026-07-28
weight: 74
author: Houming818 & Codex Review
description: "公开说明 STONE-1 C10 的条件坍缩、teacher forcing 混杂与 EOS 尾部问题，并界定哪些历史结论保留、哪些必须撤回。"
tags: [SPR, TreeHeap, ARA, STONE-1, Retraction, Teacher Forcing, Mode Collapse, NLL]
---

# 为什么 Loss 在下降，模型却只会一句话

这是一次正式勘误，不是给失败结果换一种好听说法。

C10 完成了约 `1.410B` 个目标 token 的训练，验证 NLL 从 `16.7848` 降到 `4.7275`。然而 Houming818 用三个不同的英文输入测试同一个 checkpoint：

```text
The earth is round.
The apple is sweet
why is the window wet? because the sky cried
```

三个输出都进入近乎相同的“一带一路”重复句式，而且 48 个输出 piece 内没有正常结束。这是严重的**条件坍缩**：不同英文没有产生不同答案。

因此，C10 不能作为翻译成功、STONE-1 完成或私有协议形成的证据。

## 1. 为什么输出一样，Loss 还能下降

训练和 CLI 执行了两个不同任务。训练使用 teacher forcing：

$$ p(y_t\mid y^{gold}_{<t},x) $$

预测第 $t$ 个中文 token 时，decoder 已经看到了正确中文前缀。CLI 自由生成只能读取自己的历史输出：

$$ p(y_t\mid y^{model}_{<t},x) $$

即使模型忽略英文，只要训练时看到了“苹果很”或“地球是”，仍可能猜出下一个字，NLL 因而可以下降。自由生成却从同一个 BOS 开始，一旦选中高频逗号，便可能滑进最强的中文模板。

所以 C10 的 NLL 真实记录了优化，却回答错了问题：

```text
它测到了：给定正确中文前缀，能否继续中文。
它没证明：给定不同英文，能否生成对应中文。
```

## 2. EOS 尾部淹没了英文

C10 的 TreeHeap 有 256 个可见 leaf。短句只有约 6 到 11 个有效 piece，其余位置被写成 EOS，并继续参加 FOLD。一个 6-piece 输入接近：

```text
6 个英文 piece + 250 个可见 EOS
```

不同英文句子的少量差异因此处于几乎相同的大面积 EOS 背景里。这是必须修复并定量消融的混杂变量。

## 3. 历史结论怎样处理

| 历史内容 | 新状态 | 原因 |
|---|---|---|
| C10 是有效翻译 checkpoint | 撤回 | 三个无关输入生成同一模板 |
| C10 NLL 证明英文到中文学习 | 撤回 | teacher forcing 允许忽略英文 |
| C09 已完成 STONE-1 | 暂停 | 原 gate 没有排除 source 忽略 |
| C08 可公开下载 | 保留 | 文件和复现价值仍在 |
| C08 是可靠翻译候选 | 暂停 | 需要条件依赖复审 |
| detail shuffle 会改变 NLL | 保留观测 | 数字本身仍是真实结果 |
| detail shuffle 证明英文语义进入树 | 撤回外推 | 只证明 decoder 依赖某些树状态 |
| M0 代数与 mirror/compose proof | 不受影响 | 不依赖 C10 翻译 Loss |
| 预算训练与旋转方案 | 保留为假设 | 尚未被本次结果直接验证 |

旧文章不会删除。主要受影响文章顶部增加“证据状态更新”；仍在修订中的 SPR-073 由本文统一覆盖，让读者同时看到原判断和后来的反证。

## 4. 重启训练前的四道门

### Gate A：Source shuffle

固定中文 target，只打乱英文 source。真正依赖英文的模型应满足：

$$ L_{shuffle}-L_{native}>0 $$

### Gate B：Empty source

把英文换成严格 mask 的空输入。如果 NLL 几乎不变，decoder 主要是中文语言模型。

### Gate C：First-step logits

生成第一个 token 时还没有中文历史。不同英文的第一步概率分布必须出现可重复差异。

### Gate D：自由生成体检

固定测试集必须报告：

```text
unique-output rate
source-output conditional diversity
repetition rate
EOS arrival rate
BLEU / chrF
人工样例
```

这些 gate 未通过前，不再启动几十小时的扩容训练。

## 5. 代码修复方向

1. source 可见长度只覆盖真实 token 和必要结构节点，padding 不得伪装成大量有效 EOS；
2. 保留 token 交叉熵，但加入 wrong-source 对照；
3. 每次验证同时运行 teacher-forced 与 free-running 路径；
4. checkpoint 选择不能只看 teacher-forced NLL；
5. 小规模审计通过后才能重新扩容。

可以尝试条件依赖约束：

$$ L_{dep}=\max\left(0,m+L_{native}-L_{wrong\ source}\right) $$

它只是待验证的修复候选，不是预先正确的答案。

## 6. 当前航行状态

```text
C10 长训练：完成，但翻译 Claim 无效
C09 STONE-1 完成状态：暂停
C08 公开制品：保留，翻译身份待复审
下一步：先证明 source 真正进入计算，再谈规模化
```

这次最有价值的 evidence 不是 `NLL=4.7275`，而是三个手工 CLI 输入。它们迫使审计回到训练函数，找到了 teacher forcing 与自由生成之间的断层。

公开错误是 ARA 的核心功能。数据不会删除，旧文章不会被偷偷改成“我们早就知道”，错误结论也不会继续成为下一条 Claim 的地基。

> **License: GPLv3。本文的勘误、审计方法和后续 falsification gate 与 SameTime/ARA 一并公开。**
