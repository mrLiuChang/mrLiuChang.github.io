---
layout: post
title: "The RW-Alogorithm's formula"
date: "2018-09-10 21:31:45 +0800"
categories: [Alogorithm]
tag: [Alogorithm,rw]
---
关于公式计算重点在论文的12-13页。
公式19。
首先是C的计算：
  aSet.getC()
  对fi的理解：出现了i次的文档的个数。比如出现了一次的文档1000个，出现了两次的文档100个。
  fi*(1/2)*(i*(i-1)) 然后求和（i 从 1 开始）。
  正确。


其次是文档度数的两种求和方式的乘积：
  aSet.getMultiply()
  明确文档度数是，unique words的个数，所以并不简单是文档中包含的词的个数。

下面就着手解决这两个问题。
检查发现，上面的两个公式并没有问题。

问题集中在docDegree的计算，keyword的选取上。
对于docDegree，首先需要清洗字符串，然后分词，然后去除重复，然后去除常用词。
对于keyword选取，过程和上面相同。

后面四步对字符串的操作过程，放在一个函数中，StrWash(String) 返回一个List《String》
