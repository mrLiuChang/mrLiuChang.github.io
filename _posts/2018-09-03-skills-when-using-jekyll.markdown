---
layout: post
title: "Skills when using jekyll"
date: "2018-09-03 19:31:53 +0800"
categories: tech
---
关于文章的摘要，在jekyll中用 excerpt_separator: <!--more-->来定义。<!--more -->
如果觉得自动生成的摘要不理想，可在文章中设置标志。默认的摘要为第一个
在yml中可以定义全局的分隔符，sth。

关于遍历，对于每一个categories里的category，都是一个二维的数组，category[0]代表name,
category[1]代表该category下的post对象的一个一维数组。
![jekyll-iteration](/image/jekyll-iteration.png)

关于逻辑运算符，if语句支持逻辑运算符 or 。
