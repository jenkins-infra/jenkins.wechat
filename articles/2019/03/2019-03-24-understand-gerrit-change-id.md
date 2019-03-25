---
title: "理解 Gerrit 的 Change-Id"
description: "使用 Gerrit 的第一步"
date: 2019-03-27
tags:
- gerrit
- code-review
author: zacker330
---

> 审校：LinuxSuRen（https://github.com/LinuxSuRen）

![Gerrit workflow](../../../images/gerrit/gerrit_workflow.png)

Gerrit 是一个基于 Git 版本控制的基于 Web 的代码审查工具 。笔者在学习它的过程中发现，要使用好它，第一步就是要理解 Change-Id。

### 理解 Change-Id

要理解 Gerrit 的 Change-Id，我们就必须对“一次代码审查任务”有一个定义。通常，我们认为对一次完整的功能实现或 Bug 修复（即一次完整的变更）进行代码审查是合理的。而对一个半成品进行代码审查，得到的结论是不可靠的。因此，一次代码审查任务意味着是对一次变更进行审查。

Gerrit 使用 Change-Id 来标识一次变更。Change-Id 实际上就是一串字符串，类似这样：`Ic8aaa0728a43936cd4c6e1ed590e01ba8f0fbf5b`

但是，一次变更通常会伴随多次 Git 提交（Commit），而且每次提交的提交是不同的 Commit Id（提交Id）。Gerrit 如何将多次提交关联到同一个 Change-Id 呢？

我们需要在每次提交时，将 Change-Id 以规定的格式放在提交消息（Commit message）的Footer 部分中（最后一行）。如下图：

![](../../../images/gerrit/gerrit-commit-message-with-change-id.png)

Change-Id 为避免与提交 Id 冲突，通常以大写字母`I`为前缀。但是，我们怎么才能方便生成 Change-Id 呢？

### 使用 Git 钩子生成 Change-Id

Change-Id 最好是自动生成，并放到提交消息指定位置，这样才能节约开发者的时间。Gerrit 提供了标准的“commit-msg”钩子来实现。

Git 提供了4个提交工作流钩子：pre-commit、prepare-commit-msg、commit-msg、post-commit。其中 commit-msg 钩子，会在我们执行 `git commit` 时被执行。

本质上，commit-msg 钩子是一段脚本程序，放在 .git/hooks 目录下。commit-msg 脚本可以使用 Shell、Ruby、Python 等语言实现。

Gerrit 的 commit-msg 钩子直接从 Gerrit 下载：

```shell
## 在项目目录下
curl -Lo .git/hooks/commit-msg http://<gerrit服务地址>/tools/hooks/commit-msg
chmod u+x .git/hooks/commit-msg
```

接下来，在我们执行 `git commit` 后，再执行 git log 就可以看到 Change-Id 了。

请注意，第一次 clone 代码到本地时，需要重新安装一次 commit-msg 钩子。因为它并不会被提交到版本库中。

### GitLab 也有类似的 Change-Id

在 GitLab 中，每个 Issue 都会有一个 Id。它是如何将 Issue Id 与 Commit Id 关联起来的呢？GitLab 的解决方案与 Gerrit 一样。只不过，GitLab 是在提交消息的第一行开始加入 Issue Id，格式如下：

```
<project name>#<Issue Id>: <commit msg>
```

例如：devops#151: 实现参数化构建。

接着，就可以在 GitLab 相应的 Issue#151 的详情页下看到下图内容：

![](../../../images/gerrit/gitlab-commit-id-issue-id-link.png)

### 小结

相信不少初次接触 Gerrit 的同学被 Change-Id 搞得一头雾水。希望此文能给读者带来一些帮助。

最后，可以看出，Change-Id 和 Issue-Id 本质上是同一样东西，都是变更的唯一标识，用于关联变更与代码提交。而变更Id 对于项目管理意义重大，因为它是代码指标与业务指标的连接点。

### 参考
* Gerrit 的 Change-Id 文档：[https://gerrit-review.googlesource.com/Documentation/user-changeid.html](https://gerrit-review.googlesource.com/Documentation/user-changeid.html)
* Gerrit 的 commit-msg 钩子文档：[https://gerrit-review.googlesource.com/Documentation/cmd-hook-commit-msg.html](https://gerrit-review.googlesource.com/Documentation/cmd-hook-commit-msg.html)