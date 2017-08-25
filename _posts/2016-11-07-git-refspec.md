---
title: GIT REFSPEC 重名问题解决小记
layout: post
tag: 计算机
---

# 原因

有个远端仓库版本管理混乱，计划以后按照[这个模式][success-zh]（[原文][success-en]）来管理版本。

现在的版本中同时有 v0.2.x 分支和 v0.2.x tag，当我们试图删除 v0.2.x 分支，git 回答无法完成操作。

```shell
git push origin v0.2.x --delete
> error: dst refspec v0.2.x matches more than one.
```

# 研究过程

最权威的莫过于自带的 manual 手册

## git-push(1)

`man 1 git-push` 手册中讲到了 `<refspec>` 的规则：`[+]<src-obj>:<dst-ref>`，其中：

* `src-obj` 是提交的标识（或者分支），如 `HEAD`，`master~4`
* `dst-ref` 是远端要被推送到的 ref（引用）
* `dst-ref` 为空时，使用 `src-obj` 同名的 ref
* `src-ref` 为空时，则是删除远端的 `dst-ref` 

所以删除远端分支的语法应该是写作 `git push origin :v0.2.x`，嗯，比诡异的 `push --delete` 语法看起来好理解一些。

不过我们的问题还没有解决，如果有同名的 `dst-ref` 如何解决，`ref` 按什么规则书写，如何查看？

## gitrevisions(7)

其中 **SPECIFYING REVISIONS** 一节中完整的解释了 `<refname>` 的表示，下面是我自己的理解。

* refname 是一个到 commit 的引用名称，方便记忆
* refname 的完整表示为 `refs/xxx/<refname>` 这样一个层级名称
* 我们通常用的 `HEAD`，`FFETCH_HEAD` 和 `ORIG_HEAD` 等实际上是特殊的别名（类似 shell 里的 alias）
* 一般情况我们并不使用完整的 `refs/heads/master` 名称，而只用 `master`，git 会匹配出完整的 refname。但有歧义时会提醒我们（就是本文开始遇到的问题）

这里不精确的类比一下

1. 版本库： git 管理的每次提交对象由 sha1 来标识，通过快照保存提交的内容
2. 元数据： git 提供的丰富的功能，如分支、tag等，实际上是包含了指向一次提交的元信息

手册中解释了 git 如何来匹配 refname 的全名，实质上在 `.git` 目录下，我们就能看到这些文件，文件的内容正是指向提交的 sha1 串

```
.git/HEAD
.git/refs/heads/master
.git/refs/heads/v0.2.x
.git/refs/tags/v0.2.x
.git/refs/remotes/origin/master
...
```

# 解决方法

到这里我们的问题就很好解决了：只需要用完整的 refname 进行操作就行：

```shell
git push origin :refs/tags/v0.2.x 
```

# 总结

* refname 是表示分支、HEAD、TAG 的易读的别名
* 一般无需输入完整的 refname 名称，git 会自动匹配
* 如果出现多个 refname 重名，则需要输入完整的 refname
* git push 是将本地的一个提交推送到远端，删除只是其没有本地提交的一个特例


# 参考资料

* [man git-fetch][git-fetch]
* [介绍一个成功的 Git 分支模型][success-zh]
* [A successful Git branching model][success-en]


[success-zh]: https://www.oschina.net/translate/a-successful-git-branching-model
[git-fetch]: https://www.kernel.org/pub/software/scm/git/docs/git-fetch.html
[success-en]: http://nvie.com/posts/a-successful-git-branching-model/
