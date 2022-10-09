---
layout: post
title: "Git本地已有仓库，如何推送到Github?"
date: 2022-10-07
description: "Git学习记录"
tag: Git
author: bugthree
---

[toc]

## Git本地已有仓库，如何推送到Github?
本地已经 `git init` ,并且已经有了commit 记录，如何将本地仓库推送到远端

1. 在远端仓库GitHub中建立一个仓库(最好不要添加ReadMe)

2. 在本地的该仓库下，输入如下命令，把新建的仓库添加为和本地仓库关联的默认远程仓库
`git remote add origin https://github.com/xxxx/xxxx.git`
`git remote add` 含义在于将本地仓库与远端仓具 建立一个链接，
`add`指的就是添加一个链接
`add origin ` origin 代表的是远端仓库的名字，一般都角origin，而在后面的地址，那就是远端仓库的真实地址

3. 从远程分支拉取master分支并与本地master分支合并
`git pull origin master:master`

4. 提交本地分支到远程分支
`git push -u origin master`
`push -u` 的意思是将本地的分支版本上传到远程合并，并且记录push到远程分支的默认值；
当添加“-u”参数时，表示下次继续push的这个远端分支的时候推送命令就可以简写成“git push”

注意如果上面没有添加`ReadMe`,该步可以成功执行，若添加了会出现如下错误：


To github.com:xxxx/xxxx.git
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'github.com:xxxx/xxxx.git'

此时可以强制push 到远端
`git push -u -f origin master`

5.成功后便按照之前的流程即可：

[注] `git push --set-upstream origin master` 如果设置了该命令(似乎-u 也可以)，后面就可以直接 `git push`
否则使用 `git push origin master`即可

## 记录一个github 参与开源项目的流程
1. 直接从开源项目下的`code`下 `git clone  xxxx.git`,注意并没有先`fork`到自己的远端仓库中。

2. 此时，在Github中才将刚刚的开源项目fork到自己的远端仓库中

3. `git remote -v`下，结果如下：可以发现此时 显示的是开源项目的地址

```

$ git remote -v
origin  git@github.com:xxxx.git (fetch)
origin  git@github.com:xxxx.git (push)

```

4. 此时在本地，输入`git remote set-url origin git@github.com:xxx.git` origin 后的地址为自己fork后的远端地址，也就是自己远端仓库下该项目的地址
- 其中 set-url命令修改remote URL
- git remote set-url传递两个参数
  - remote name。例如，origin或者upstream
  - new remote url。例如，git@github.com:USERNAME/OTHERREPOSITORY.git


5. 此时在`git remote -v`下，地址就变到自己的远端仓库了

```

$ git remote -v
origin  git@github.com:xxxx/xxxx.git (fetch)
origin  git@github.com:xxxx/xxxx.git (push)

```

6. 但是，我们还需要从开源项目中`pull`最新的代码：所有必须在设置下开源项目的地址(也就是设置上游仓库)
`git remote add upstream git@github.com:xxxx.git`

7. 此时在`git remote -v`下，结果如下
```

$ git remote -v
origin  git@github.com:xxxx/xxxx.git (fetch)
origin  git@github.com:xxxx/xxxx.git (push)
upstream        git@github.com:xxxx.git (fetch)
upstream        git@github.com:xxxx.git (push)

```

8. pull 开源代码时，使用`git pull upstream master`
网上看到的方法是：
- 拉取upstream branch及commit
  - `git fetch upstream`

- 切换到主分支
  - `git checkout master`

- 合并到本地分支
  - `git merge upstream/master`

- 推送到origin代码库(origin repository:指从开源项目Fork出来的远程代码仓库，也就是自己fork的远端项目仓库)
  - `git push origin master`
