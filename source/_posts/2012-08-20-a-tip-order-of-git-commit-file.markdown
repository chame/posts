---
layout: post
title: "a tip order of git commit file"
date: 2012-08-20 14:46
comments: true
categories: [work, git, commit]
---

---
## git commit的顺序小记
---

我在本地从master分支中新建了一个关于此模块的分支 `test_job`

    git checkout -b test_job

此命令会自动在本地创建`test_job`分支，并从本地分支切换到`test_job`分支的。

然后在此分支上做了许多的工作，当想提交的时候，我想了些。此前并不意识到，可能的与其他代码人员的代码冲突，结果往往另我很伤神。

这次应该多检查一下才行。

首先，查看状态。

    git status

很不幸，其中有一个文件是我没想修改的。但是不知道什么时候，可能是我的原因或者编辑器的原因，对这个文件创造出了修改。这里称此文件为 `app/model/other_job_model.rb`。

好吧，虽然说，我是不想改这文件的。但是，刚过周末，会不会是上周的一个小需求需要修改此文件，而这周的我忘却了呢？那先具体看看此文件有哪些改动吧。

    git diff HEAD -- app/model/other_job_model.rb

此命令会显示文件`app/model/other_job_model.rb`最后一次提交与当前代码的区别。这里的最后一次提交应该是从`master`迁出的最后一次提交。

>        $ git diff test            (1)
>        $ git diff HEAD -- ./test  (2)
>        $ git diff HEAD^ HEAD      (3)
>
>    1. Instead of using the tip of the current branch, compare with
>    the tip of "test" branch.
>    2. Instead of comparing with the tip of "test" branch, compare
>    with the tip of the current branch, but limit the comparison to
>    the file "test".
>    3. Compare the version before the last commit and the last
>    commit.

看了之后顿时无语，不知道什么时候手贱，对此文件删除了一空白行。而　git 神经紧张地报告了这一修改。好吧，至少我还是不希望这一修改提交的，虽然对他人的代码的运行不会有问题，但是，未知的东西太多了。我小白无知的太多了，别动他人代码为好。

    git checkout -- app/model/other_job_model.rb

相关语法：
> git checkout [-p|--patch] [<tree-ish\>] [--] [<paths\>...]

重新查看状态

    git status

好了，此时的文件应该是我自己要修改的文件了。可以提交了。

    git add ./  #如果有删除或者更多非添加操作，可以　git add -A
    git commit -m "test_job ok"

好了，此版本不会涉及到他人文件了。现在把此分支提交到远程吧。

    git push origin HEAD

完成，远程会自动创建`test_job`这一分支并包含刚才push的代码的。

