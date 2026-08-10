---
title: "Verifiable Research?"
date: 2026-08-11
mathjax: true
categories:
  - "Science"
---
近期，笔者在同时考虑 AI4SCI 和 AI4AI 的 benchmark 应该怎么做。事实上笔者一直认为 AI4AI 应该被视为 AI4SCI 的一个子集，因为：

1. AI 是 Science。关于 AI 的知识目前普遍是经验性的，但这并不意味着 AI 不属于科学，科学可以是经验的；同时我们也期望着它可以更定量（physics of AI）。
2. 研究模型是在一个完全可控的 universe 中探索（in principle，每个参数都受到研究者控制、在其观察范围之内）（by ziming），因此某种意义上是所有 AI4SCI 中最可做的一支。

所以本文将把 4SCI 和 4AI 的 benchmarking 放在同一框架下思考。

笔者的出发点是，一个好的 benchmark 需要具备的性质包括但不限于：

1. 可验证；
2. 够真实；
3. 有创新；
4. 够难。

这里 1 和 3 似乎很多时候是矛盾的。事实上很多 benchmark 都在 3 的边界游走，因为 1 需要定义一些 rubrics，但是往往 rubrics 写的越清楚就越像竞赛题（例如 frontier science 的 research subset 我觉得就和 olympiad subset 没有实质区别）。

笔者能想到的几个保证可验证性（1）的路径：

- 只看模型 loss，同一个 setting 哪个 agent 优化到的 loss 低就牛逼。
- 算力不对称，大算力获得可验证结果，但 agent 没办法直接模拟。4AI 和 4physics 都能做，后者用 simulator 环境。
- 反向设计：设计出具有某个性质的系统，环境验证这个性质。

笔者能想到的保证创新性（3）的路径：

- 做在线 bench，搜 open problem；未必很难，但是要解决它必然有旧知识到新知识的整合过程。[^2]
- Simulator 环境如果能跑出新的现象，就自动成为一个 valid research problem。
- 还有就是专家参与（例如 top researcher 和 agent 的对话流，应该是一个非常高质量的数据）。参考很多 AI4SCI 已有的造题 pipeline 还是有专家参与，合成并不是太成功。[^1]

看起来，两者重合的地方在 simulator as judge 这个路线上。

## 两种边界

如果真的要做 autoresearch，最终的大目标肯定是让模型输出「human knowledge 边界」之外的东西。可是现在我们的目的是评测，假如我们知道如何对这样的输出写 rubrics，其实我们或多或少预设了模型的目标。

然而我们如果退而求其次，把 autoresearch 的能力弱化为「发现自身已有知识之外的新知识」，这就给出了一些做评测的空间。例如「用爱因斯坦之前的知识 pretrain，然后测试模型是否能提出相对论」，实际上就是在测评这样一种弱化了的能力。这样一种能力并不是 trivial 的，独立发明前人发明过的东西，在智力上是很有挑战性的，而且正是很多 top researcher 曾经做过的事情，

换言之，我们不再要求模型突破「human knowledge 边界」，只要求它能突破「pretrain knowledge 边界」。这正是解读 simulator as judge 的另一种视角。

## 难度来源

一个需要注意到的事实是：ground truth 的生成依赖算力，并不意味着问题就是困难的。如果合成题目的流程使用了模型，需要小心地保证题目的难度来源于外部 compute 真的引入了「知识边界以外的信息」，而不是仍然来源于模型本身「把这道题设计得很好」。所以，也许理想情况下，应该先用外部 simulator 做足够多的实验、发现非平凡的现象，再以「解释或预测该现象」为题目的内容；这里注意到「依赖外部计算」只是一道题非平凡的必要但不充分条件。

一个 side note 是，在笔者制备题目的过程中，会发现：笔者使用的 coding agent 的基础模型，在测评时的表现也倾向于更好。可以确认，这并不是因为 ground truth 泄漏，但这种相关性似乎是存在的。「这种相关性是否具备普遍性」是笔者认为值得深入研究的独立问题，尤其是对于关心 posttrain 数据质量的人。



[^1]: 但是由于 AI4AI 具有完全可控的性质，合成也许是 promising 的。这就是 ArchitectureIQ 的基本想法。

[^2]: 但是「搜集 open problem 并尝试用模型攻克之」是有趣的，笔者最近也在做相关的尝试。
