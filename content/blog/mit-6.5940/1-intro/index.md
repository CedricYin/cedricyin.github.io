---
title: intro
date: 2024-05-17
tags:
  - MIT-6.5940
  - AI
  - AI Sys
summary: 概述了高效深度学习系统尤其是模型压缩技术的必要性，列举了近期热门的大模型应用
image:
---
## 课程概述

课程前置知识
- 计算机体系结构
- 机器学习基础

课程概览
- 高效推理
	- 剪枝和稀疏化
	- 量化
	- NAS
	- 知识蒸馏
	- MCU Net
	- TinyEngine
- 特定模型的原理及优化
	- transformer & llm
	- vit
	- gan，video，point-cloud
	- diffusion
- 高效训练
	- 分布式训练：模型并行，数据并行，流水线并行
	- on-device training
- 量子机器学习

Labs：在colab完成
0. 熟悉pytorch
1. pruning
2. 量化
3. NAS
4. LLM压缩
5. 部署LLM在个人电脑

## Intro

硬件的计算能力的发展和模型的size的发展存在**gap**，导致模型压缩技术显得很重要
- 模型压缩技术（再加个并行计算）可以说是**ai应用（需要算力）和ai硬件（提供算力）之间的桥梁***

模型压缩技术同时也**会影响一些硬件加速器的架构设计**，使这些硬件加速器能直接运行压缩过的模型

模型压缩的需求还源自于在一些**资源有限**的设备上进行推理，甚至是**on-device training**

压缩过的模型的推理结果意味着质量的降低，比如生成图片时，压缩模型生成出来的图片质量肯定不如原模型来得高，但往往都是可以接受的。—— 需要在速度和质量做一些**tradeoff**

LLM的**提示工程**相关的知识
- **zero-shot，one-shot，two-shot**对应给LLM正确**示例**的数量，shot越多意味着LLM之后的推理准确度会越高，但同时推理会越慢（因为输入变多了）
- 思维链**chain-of-thought**：给LLM的正确示例的prompt中，正确答案要附上理由，告诉LLM为什么答案是这个
	- 这个prompt技巧可以大大提升LLM的推理准确性
	- 使LLM能做到chain-of-thought的前提是，**模型必须足够大，参数必须足够多**，以达到**涌现**emergent**



