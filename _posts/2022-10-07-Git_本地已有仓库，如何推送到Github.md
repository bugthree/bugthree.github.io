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

## 总结参与开源项目必备的GitHub工作流程
- Remote(远端仓库)
  - main(master) （fork后开源项目后自己的远端仓库）
  - upstream (开源项目的远端仓库)
- Local(本地仓库)
  - main(master)

### 工作流程
1. 将`Remote` 仓库 `git clone` 到`Local`
2. 新建一个分支 `git checkout -b my-feature` 复制当前分支(master)到新分支，名字叫`my-feature`
  - 建分支的好处：1）隔离主分支 2）利于多人合作
3. 进行开发，开发完建议使用`gif diff`,查看当前分支与master分支有何区别？
  - git add
  - git commit -m ""
4. 使用`git push origin my-feature` 将本地代码推送到远端
5. 假设开源项目又有了`update`,我们需要同步代码
6. 首先，在本地`git checkout master` 切换到主分支，注意此时的主分支与`my-featur`分支是不同的
7. 使用`git pull upstream master` 将开源项目远端同步到本地，使用`git pull origin master` 将自己仓库的master分支同步到本地
8. 切换到`git checkout my-feature` 该分支此时的状态，并非将远端代码同步到本地的代码,而是4)步的状态
9. `git rebase master` 将`my-feature`的修改丢到一遍，然后将master最新的修改同步到`my-feature`,然后在将4）步的修改同步过来，注意这个时刻可能会有冲突。
  - 这个过程有点类似：`git stash` 存储脏代码
  - 同步代码，`git stash pop` 将最近的脏代码同步过来
10. 假设继续开发， 开发结束，需要将本地修改同步到GitHub远端的`my-feature`仓库，此时可使用`git push -f origin my-feature`
11. 假设我们觉得我们的代码没有问题，可以合并到主分支(master)，此时可以发起一个`New pull request`
12. 合并请求，会有检视代码，如果没问题，会进行`Squash and merge` 这个操作代表，将`my-feature`分支的所有commit合并一个Commit提交上来，因为在个人功能分支`my-feature`上，会有很多很乱的commit，主分支并不希望有这么多commit，更喜欢`master commit history `更见简洁明了
13. 一般这个时候远端的`my-featur`这个分支就可以删除掉了。GitHub有`delete branch`操作
14. 此时本地还有`my-featur`这个分支，首先切回到主分支`git checkout master`
15. 使用`git branch -D my-feature`删除本地分支
16. 使用`git pull origin master` 或 `git pull upstream master` 将最新的代码同步到本地