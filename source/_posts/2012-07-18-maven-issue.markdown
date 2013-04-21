---
layout: post
title: "maven_issue"
date: 2012-07-18 15:01
comments: true
categories: 
---

mvn help:effective -Doutput=a.txt

编译出问题了，是jar包的问题么？有可以是maven下载过程中出的问题。
删除相关jar包，问题解决.
分析原因，有可能是公司网络太慢。导致下载文件时出错。。。只是猜测。
