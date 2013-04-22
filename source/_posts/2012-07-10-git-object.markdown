---
layout: post
title: "Git对象模型"
date: 2012-07-10 21:17
comments: true
tags: Git
---


接触git有段时间了,但一直没花太多时间去具体研究它,只会简单的基本日常操作.平时都是用TortoiseGit  .之前买了<Git权威指南>没怎么看,现在是看的时候了.

说Git对象模型之前,首先要说的是:

>    Git与你熟悉的大部分版本控制系统的差别是很大的。也许你熟悉Subversion、CVS、Perforce、Mercurial 等等，他
>    们使用 “增量文件系统” （Delta Storage systems）, 就是说它们存储每次提交(commit)之间的差异。Git正好与之相
>   反，它会把你的每次提交的文件的全部内容（snapshot）都会记录下来。这会是在使用Git时的一个很重要的理念。

我一开始很不理解,记录每次提交的全部内容,多浪费空间呀.后来仔细看了下对象模型,Blob对象,tree对象,commit对象.发现记录全部内容不一定会浪费空间.

可以做个实验,我把progit.pdf下载后add到git控制之中,然后提交.progit.pdf大约3.6M. .git文件大小也大约3.6M.之后复制progit为20份.工作空间约为77.3M,但提交后,.git文件夹大小还是约3.6M.查看一下.git/objects/ 只有ba开头的文件夹相对比较大,里面有个文件3.6M,应该就是progit.pdf的blob对象.

通过查看commit的sha值:

	$ git cat-file -p 52e2fb982187ecd774bf8d0d87551e8921caf77f
	tree 8a3f55987eca0fdb05568638991840dc6314060b
	author ...
	committer ...

	init
	
	$ git cat-file -p 8a3f
	040000 tree df6fda95766fede9af86eefe5016aeea0abf3260    test
	
	$ git cat-file -p df6f
	100644 blob bacc07541f5d268f0acdc736ebc35f93d323ec60    Copy (10) of progit.pdf
	100644 blob bacc07541f5d268f0acdc736ebc35f93d323ec60    Copy (11) of progit.pdf
	100644 blob bacc07541f5d268f0acdc736ebc35f93d323ec60    Copy (12) of progit.pdf
	100644 blob bacc07541f5d268f0acdc736ebc35f93d323ec60    Copy (13) of progit.pdf
	100644 blob bacc07541f5d268f0acdc736ebc35f93d323ec60    Copy (14) of progit.pdf
	
可见,tree对象只是引用了blob对象(progit.pdf)的sha1值,所以记录全部snapshot也并不一一会浪费空间.

参考资料:

[ProGit](http://progit.org/book/zh/ 'ProGit')

[Git权威指南](http://www.worldhello.net/gotgit/ 'Git权威指南')

[Git Community Book 中文版](http://gitbook.liuhui998.com/ 'Git Community Book 中文版')

