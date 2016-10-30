#  RNN与LSTM学习总结

------

> * RNN是什么
> * RNN要解决的问题
> * 标准RNN存在的问题
> * LSTM基本原理
> * LSTM的其它变体
> * RNN在目标检测方面的应用

## RNN是什么
RNN全称是Recurrent Neural Networks, 循环神经网络，近几年来在深度学习领域内的研究热度毫不逊与卷积神经网络CNN，尤其擅长挖掘时序或具有序列意义的数据中潜在的模式，在语音识别、机器翻译、句法分析等领域发挥了巨大的作用，同时在图像视频处理方面，也有较好的表现。而LSTM是指长短期记忆网络，属于一类具有更多优点广泛使用的RNN网络，除了LSTM之外还有GRU等更多变种。在这里单独就LSTM原理做一下总结。

学习LSTM一般都是从RNN基础开始，最经典的资料莫过于多伦多大学的Alex Graves的RNN专著《Supervised Sequence Labelling with Recurrent Neural Networks》以及提出LSTM的作者Felix Gers的博士论文《Long short-memory in recurrent neural networks》,这两个文献内容都有100多页还没有看，而是看了2015年UCSD的Zachary C.Lipton写的一篇综述文章《A Critical Review of Recurrent Neural Networks for Sequence Learning》以及Colah的一篇blog材料 《Understanding LSTM Networks》。

## RNN要解决的问题
人类并非时刻都从空白的大脑开始学习和认知事物，当你阅读这篇文章时，你就是在运用以前对这些词句的含义理解等先验知识来作出认知判断，就是说神经网络应当具有信息记忆能力，也就是当前时刻的输出除了与当前输入有关，还与之前的输出有关。形象地图示表示为下图左边这样，按序列展开为右边这样。
![RNN Model](http://cogito2012.github.io/assets/img/RNN.png)
那么问题来了，传统的马尔科夫链比如HMM就是解决这种具有时序依赖关系的模型啊。但是研究人员发现，当可能的隐层状态空间增长较大时，状态转移矩阵计算量呈指数式增长，使得处理的问题规模十分有限，无法处理长时间或长状态序列前后依赖的问题。

## 标准RNN存在的问题
RNN的关键点之一就是将先前的信息连接到当前的任务上对当前内容进行理解，例如预测句子“The clouds are in the   ”中的横线处，很容易根据序列中前面的clouds预测到后面横线处填sky，这种前后信息的位置间隔仍然不算长，但当场景复杂一点后，比如预测句子“I grew up in France…(此处省略N多字)…I speak fluent   ”，横线处的预测就需要依赖前面很远位置的信息“France”来推断此处为“French”。理论上RNN是可以处理这种长期依赖问题的，然而标准的RNN为了处理这样的场景，在进行模型训练时由于每个RNN单元都要循环多次，利用梯度下降算法极易出现梯度的消失问题。于是，LSTM解决了这样的问题。

## LSTM基本原理
LSTM单元设计了类似于水阀门的一种“门”结构来控制网络的信息流，实现信息的长期记忆和短期激活，从而解决长期依赖问题。LSTM单元包括输入节点、输入门、内部状态、遗忘门、输出门组成，如图所示为一个基本的LSTM单元的示意图。
