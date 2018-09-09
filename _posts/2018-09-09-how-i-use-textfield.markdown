---
layout: post
title: "How I use textField"
date: "2018-09-09 07:27:11 +0800"
categories: [lucene,index]
tag: [lucene,index,textField]
---
下面这一段官方文档说明了StringField的弊端。
>A field that is indexed but not tokenized: the entire String value is indexed as a single token.<!--more--> For example this might be used for a 'country' field or an 'id' field. If you also need to sort on this field, separately add a SortedDocValuesField to your document.

构造函数（1）
>StringField(String name, BytesRef value, Field.Store stored)
Creates a new binary StringField, indexing the provided binary (BytesRef) value as a single token.

构造函数（2）
>StringField(String name, String value, Field.Store stored)
Creates a new textual StringField, indexing the provided String value as a single token.

接着，我决定做出尝试：不使用IKAnalyser，转而使用StandardAnalyser,点击运行按钮，索引就哗哗如流水似的出来了！
但是，我在想，这样使用StandarAnalyser建立起来的索引，是否会影响到我后面的检索呢？这是第一个问题。

第二个问题是，当前虽然过滤了大部分的标点符号，但是还有一种?（属于英文符号）无法过滤掉，还有大量的数字和英文，甚至还有一些日文韩文拉丁文，这些是否都应当从索引中去除呢？

求人不如求己。

对于第一个问题，我决定先进行检索尝试。
对于第二个问题，我决定对于所有的文本一律只保留中文，对于非中文的一切字符，我全部替换成空格。
在上述两个问题解决之后，我将用我新建立的索引进行检索尝试。

此刻索引正在建立中...

再一次遇到问题了。。。
我建立的索引检索不出来啊。这是为什么呢？索引的大小是可以正确返回的啊。我尝试使用单个汉字的检索，索引可以运行。
问题在于，拿到的keyword有空串。也有是大量两个字的词汇检索不出来的。

问题再次回到了索引的建立上边。理论上来说，我在建立索引的分词器不可以用StandardAnalyser.于是我换用了SmartChinesAnalyser,程序成功运行。
