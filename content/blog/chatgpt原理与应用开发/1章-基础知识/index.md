---
title: 大模型前置基础知识
date: 2024-05-17
tags:
  - ChatGPT原理与应用开发
summary: 大模型的一些前置知识，主要是NLP基础
image:
---

## 1 从NLP到大模型发展历史

图灵测试
- 是什么？简单讲就是评判**机器是否像人一样智能**的一种测试
- 是NLP技术的**目标和驱动力**

## 2 语言模型基础

分词：即把一段文本分割成一个个token
- **token的粒度**是一个词，还是词组，亦或是句子，视具体任务而定

如何将文本表示成计算机可理解的形式？
- 词袋模型BOW，bag of words
	- 维度过大，且实际上很多维都会是0（稀疏向量），太浪费存储
- 词向量（词嵌入）：主流的表示方法
	- embedding技术：将文本转化成**稠密向量**的技术

语言模型是什么？**输入给定文本，能输出预期的文本**的模型

## 3 大模型基本原理