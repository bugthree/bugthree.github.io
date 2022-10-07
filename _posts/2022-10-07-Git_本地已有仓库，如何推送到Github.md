---
layout: post
title: "Git本地已有仓库，如何推送到Github?"
date: 2022-10-07
description: "Git学习记录"
tag: Git
author: bugthree
---

[toc]

# Git本地已有仓库，如何推送到Github?
本地已经 `git init` ,并且已经有了commit 记录，如何将本地仓库推送到远端

1. 在远端仓库GitHub中建立一个仓库(最好不要添加ReadMe)

2. 在本地的该仓库下，输入如下命令，把新建的仓库添加为和本地仓库关联的默认远程仓库
`git remote add origin https://github.com/xxxx/xxxx.git`

3. 从远程分支拉取master分支并与本地master分支合并
`git pull origin master:master`

4. 提交本地分支到远程分支
`git push -u origin master`
注意如果上面没有添加`ReadMe`,该步可以成功执行，若添加了会出现如下错误：

```

To github.com:xxxx/xxxx.git
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'github.com:xxxx/xxxx.git'

```

此时可以强制push 到远端
`git push -u -f origin master`

5. 成功后便按照之前的流程即可，
`git push --set-upstream origin master` 如果设置了该命令，后面就可以直接 `git push`,否则使用 `git push origin master`即可。