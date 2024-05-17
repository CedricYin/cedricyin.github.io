---
title: 李宏毅：ELMO、BERT、GPT
date: 2024-05-17
tags:
  - LLM
summary: ELMO、BERT、GPT的基本原理
image:
---
[资源](https://www.youtube.com/watch?v=UYPa347-DdE&list=PLJV_el3uVTsOK_ZK5L0Iv_EQoL1JefRL4&index=62)

下面的内容主要是一些简单的直觉上的理解，具体的模型架构和算法需要去看别的参考资料或论文

---

## ELMO

ELMO：一种**生成embedding**的技术（一种获取单词的分布式表示的技术），可以区分一词多义，实现**相同词在不同含义下的embedding不同**
- 基于**RNN**

## BERT

BERT，Bidirectional Encoder Representation from Tansformer（就是只用了编码器的transformer）
- 也是一种**生成embedding**的技术，和ELMO一样**支持一词多义**
- 不需要标签数据，只需要一堆输入，即可训练（无监督）
- 如何训练？随机mask掉一些输入，当作标签
	- 方法1：经过bert层后（transformer的编码器）将mask变量对应的embedding输入到一个分类器，**分类目标是被mask的值**![](Pasted%20image%2020240517155715.png)
		- ERNIE：一种**适用于中文**的方法1。**mask的粒度可以是中文的一个词**，而不单单是字
	- 方法2：预测两个句子是否能接在一起，然后将开头输出的向量传入一个二分类器 ![](Pasted%20image%2020240517160352.png)
	- 方法3：上面两种一起用
- 如何使用bert？<mark>微调（fine-tune）</mark>预训练好的bert模型 + 从头开始训练特定任务下的模型，二者叠加起来完成特定的任务
	- 具体来说，就是在上面介绍的bert模型的基础上，叠加一个模型
		- 进一步理解：预训练好的bert已经可以**很好地表示各个token的embedding**，因此**下游任务**的输入文本可以被bert很好地表示出来，在此基础上根据特定场景训练出来的模型的准确度会很高
	- 以**文本情感分析**为例，下面的bert**微调**即可，上面的分类器需要**从头开始训练**![](Pasted%20image%2020240517162224.png)
- 现在出现了各种各样的bert，这些bert内部有**非常多的层（常见的有24、48层）**（即参数非常多），包括**多语言bert**，即使用了各种语言进行训练的bert
	- 多语言bert的神奇功能：向上面的使用方法那样，喂它英文资料，让它学习英文文献分类。**微调好后，该模型对中文文献也能分类！**![](Pasted%20image%2020240517163110.png)

## GPT

GPT，Generative Pre-Training
- 其实就是**transformer的解码器**（recall：**解码器是用来生成文本的**）
- 顾名思义就是预训练的生成式模型，换句话说，GPT就是一个**预训练好的语言模型**，由于其参数量之大，简称**大语言模型LLM**
	- ELMO的参数量可能不过百MB，bert的参数量可能只有几百MB，**GPT的参数量一般都是以GB为单位的**
- 利用GPT，可以在**不用任何训练**的情况下，就完成许多NLP任务
	- bert还需要微调和训练，gpt直接不用训练