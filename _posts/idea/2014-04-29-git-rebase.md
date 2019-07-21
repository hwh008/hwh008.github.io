---
layout: post
title: 'git rebase 实践'
date: 2014-04-29
author: huang
cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: git
---


rebase可以这么理解，之前创建分支后做了很多提交，现在尝试根据最新的主干重新创建分支，然后提交相同的内容。
这么做的好处是，保持主干提交的连贯性，方便其他人进行merge。

ladder-web拥有两个分支：

 1. master
 2. create-pac
 
现在的任务是将*create-pac*分支rebase后，merge到*master*并提交。

首先确保*master*分支最新：
`git checkout master`
`git fetch`
`git merge origin/master`

然后开始rebase：
`git checkout create-pac`
`git rebase master`

在这里我遇上了好几次冲突，主要是db/schema.rb的。因为我知道schema可以重新生成，所以当时选择了*rebase --skip*。
我以为skip是忽略冲突的意思，没想到结果是直接放弃了这个提交，导致rebase完成后，缺少了好几个文件，测试也无法通过。

麻烦大了，怎么修复？祭出reset神器。
`git reflog`
`git reset HEAD@{10}`
`git clean -df`

reflog指令是查看git的提交记录，找到rebase开始前的那个节点，然后将HEAD指针reset回去。
reset后，工作目录下可能会残留一些rebase加进来的unchecked文件，用clean指令删除。

从这个问题学到两点：

 1. rebase处理冲突的时候，不要使用skip，除非真的无法解决冲突
 2. db/schema.rb不需要加入版本管理，完全可以重新生成
 
重新执行rebase指令，解决所有冲突后，成功。然后需要跑测试校验一下：
`rake db:drop`
`rake db:migrate`
`rake test:prepare`
`rake spec`

最后merge到主干并提交
`git checkout master`
`git merge create-pac`
`git branch -d create-pac`
`git push`

在merge到主干后，同样需要跑一次测试。