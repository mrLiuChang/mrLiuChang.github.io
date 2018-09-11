---
layout: post
title: "How to Use Smart Chinese Analyser"
date: "2018-09-09 15:26:58 +0800"
categories: [analyser,lucene]
---
ansj分词结果出现了大量的单字词，并且出现了很多词检索不到的问题，这是为什么呢？
或许是因为，我在建立索引的时候用的是smartChineseAnalyser,但是在检索过程中，使用了
ansj。下一步我继续尝试，在检索过程中使用smartChineseAnalyser。
在更换了之后，终于实现了百分之百的keyword命中。

然而，我在从文档中选取keyword的时候，用的仍然是ansj有可能是这一点导致了，当前的估测结果远远
超出了预期的结果。我于是试图修改取词方式，为smartChineseAnalyser。

在找了一番未果之后，我决定放弃修改，仍然使用ansj取词。因为使用ansj取词，未命中率极低。
那么抽不准的问题在哪里呢？
下面要开始问题检查，把整个代码的逻辑框架理一理。
1. 对照论文检查公式是否计算错误，有必要把公式写出来。
2. 顺序查看数据结构是否有误。是否能进行拆分和优化。
3. 优化分词方式。从索引的建立，到查询的建立，再到文档的取词。这些地方都是需要分词器的。

其中，1和2必须在今天晚上完成。

2000次抽样的预测结果：
================================================================
程序运行时间： 3.1666334min
num of documents:1988
the C now is 13
the estimated population is 334496

n=3000
================================================================
程序运行时间： 4.8357min
num of documents:2982
the C now is 18
the estimated population is  521333

n=4000
================================================================
程序运行时间： 6.932033min
num of documents:3954
the C now is 46
the estimated population is 366764
