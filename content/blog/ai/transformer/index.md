
学习资源：
- [transformer详解](https://jalammar.github.io/illustrated-transformer/)
- [李宏毅讲解](https://www.youtube.com/watch?v=ugWDIIOHtPA&list=PLJV_el3uVTsOK_ZK5L0Iv_EQoL1JefRL4&index=61)
- [https://zhuanlan.zhihu.com/p/338817680](https://zhuanlan.zhihu.com/p/338817680)

简单理解：

Transformer是一种seq2seq模型，主要使用了**self-attention**机制，解决了传统的RNN模型无法并行的问题。其中self-attention通过使用**QKV**三个矩阵将时序信息很好地融合到输出，使得输出结果考虑了原始数据中的时序，同时，由于都是矩阵计算，因此能够支持**并行加速**，不需要像RNN那样按序输入，顺序执行。

**多头**自注意力机制是什么？由多组QKV组成，每一组QKV可以考虑不同的信息。

**位置信息**，在embedding层后需要对输出向量的每个元素加上一个人工标注的位置信息，使得保留时序性。（为什么要加？因为自注意力机制本质上不考虑时序）另外，位置信息是**非学习参数**

![](Pasted%20image%2020240516154540.png)