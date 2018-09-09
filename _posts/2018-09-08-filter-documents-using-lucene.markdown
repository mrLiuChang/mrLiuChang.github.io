---
layout: post
title: "Filter Documents Using Lucene"
date: "2018-09-08 14:35:14 +0800"
categories: "lucene"
tag: [Index,Filter,Documents]
---
# Next I will filter the dots and renew a clean Index.
<!--more-->
我过滤字符串标点符号的正则表达式是：
'''java

	tempStr=tempStr.replaceAll("[\\p{P}+~$`^=|<>～｀＄＾＋＝｜＜＞￥×]", " ");

'''

为了查看Read中的域有哪些，我用到的代码是：

'''java
for (LeafReaderContext subReader : Reader.leaves()) {
    Fields fields = subReader.reader().fields();
    for (String fieldname : fields) {
      System.out.println(fieldname);
    }
}
'''

下面的问题是：用现在新的"body"域的字符串去替换原来的的"body"。
手头的Index有3个field，分别是body,docno,以及url。我的索引中只需要一个body域即可。

在官方API找到了索引的建立方法：
>https://lucene.apache.org/core/6_6_1/demo/src-html/org/apache/lucene/demo/IndexFiles.html
>
但是在建立索引的运行过程中，发现StandardAnalyser只是按照空格去分词，这不适用于中文，所以，要选取另外的中文分词器。

>      Analyzer analyzer = new StandardAnalyzer();

问题出现在上处。在导入了google IK
中文分词包之后，上述问题成功解决。但是，报错依然出现：
>Document contains at least one immense term in field="body" (whose UTF8 encoding is longer than the max length 32766), all of which were skipped.  Please correct the analyzer to not produce such terms.  The prefix of the first immense term is: '[-17, -69, -65, -25, -67, -111, -24, -76, -83, -25, -78, -66, -27, -109, -127, 32, -25, -67, -111, -24, -76, -83, -25, -78, -66, -27, -109, -127, -27, -101]...', original message: bytes can be at most 32766 in length; got 109582

于是需要继续去想办法解决这个问题。根据错误描述，应该是“body”中包含了超大的term导致的，编译器建议修正分词器。

在我换用
>	org.apache.lucene.document.TextField bodyField = new org.apache.lucene.document.TextField("body",tempStr, Field.Store.YES);
之前，编译器爆出了Unresolved compilation problem: 报错并未指明错误
的具体位置。

很快我找出了这个问题的解决方案，受到CSDN某位博主的启发，博主是这样说的：
>，有一个import出现了问题，因为之前改了一些东西，import里面没有自动更改。虽然这个错误与list无关，但是这样就导致整个类文件出问题，最终就会出现编译错误。

具体到我的问题，在于对于TextField对象，不仅是org.apache.lucene.document.TextField类里面有，而且有的jdk内置类里也有，这样很显然就造成了冲突。

紧接着，我又遇到了这样的一个错误：
>java.lang.AbstractMethodError: org.apache.lucene.analysis.Analyzer.createComponents(Ljava/lang/String;)Lorg/apache/lucene/analysis/Analyzer$TokenStreamComponents;

错误提示是AbstractMethodError，对于这样的一个错误，starkoverflow上提示是Abstract Method Error只会在编译期间被调用，而不会在运行期间被调用。

于是我继续试验原来的StringField发现仍然是出现上面的报错。
针对这样一个问题，我决定花时间深入研究StringField 和TextField两者的区别。
