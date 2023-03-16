# Git之Tag

tag 是 git 版本库的一个标记命令，指向了某个 **commit** 的指针，tag命令主要用于**发布版本管理**，当一个版本发布时，打赏tag标签。
可以把tag想象成一个保存的位置或标记，方便后面对该版本进行追踪和管理。

tag相当于是一个**快照**，是**不能更改它的代码**。如果要在 tag 代码的基础上做修改，需要新建一个分支`git checkout -b 分支名称 标签名称`，这叫**检出**。

**要是已创建了一个标签（已上线了），但是线上有问题，修改这个问题要再创建一个标签吗？**

## tag 与 branch 的区别

| 分支           | 区别                                                         |
| -------------- | ------------------------------------------------------------ |
| tag（标签）    | tag 对应某次 commit，是一个点，不可移动                      |
| branch（分支） | branch 对应一系列的 commit，是很多点连成的一根线，有一个 HEAD 指针，是可以依靠 HEAD 指针移动的 |

所以，两者的区别决定了使用方式，改动代码用 branch，不改动只查看用 tag。tag 和 branch 的相互配合使用，有时起到非常方便的效果，例如：已经发布了 v1.0 v2.0 v3.0 三个版本，这个时候，我突然想不改现有代码的前提下，在 v2.0 的基础上加个新功能，作为 v4.0 发布。就可以检出 v2.0 的代码作为一个 branch ，然后作为开发分支。

## 分支命名规范

| 分支           | 命名规范                                                     |
| -------------- | ------------------------------------------------------------ |
| tag（标签）    | 因为 tag 主要用来**发布版本管理**，所以其名称规范是**版本号**，例如：1.0.0、1.2.0、1.3.0、v2.1.0 等 |
| branch（分支） | branch 分支是用来开发的，其名称规范尽量清楚，格式：**类别 + / + 日期/迭代版本号/功能名称**，例如：feat/1.0.0、fix/1.0.0 等 |

- 功能（feature）分支 ，例如：feat/2.1.1，feat/user_manage_1.1.1
- 修补bug（fixbug）分支 ，例如：fix/20201214，fix/user_manage_20201214

## git fetch

拉取远程仓库分支和标签，更新本地的分支和标签

## 标签类型

分两种：（1）轻量标签；（2）附注标签。

| 标签类型 | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| 轻量标签 | 只是某个 commit 的引用，可以理解为是一个 commit 的别名       |
| 附注标签 | 是存储在 git 仓库中的一个完整对象，包含打标签者的名字、电子邮件地址、日期时间以及其他的标签信息，它是可以被校验的，可以使用 GNU Privacy Guard（GPG）签名并验证 |

## 创建标签

| 标签类型 | 创建标签                                                     |
| -------- | ------------------------------------------------------------ |
| 轻量标签 | `git tag 标签名`（直接给当前的提交版本创建一个轻量标签）OR   `git tag 标签名 提交版本`，给指定的提交版本创建一个轻量标签 |
| 附注标签 | `git tag -a 标签名称 -m 附注信息`（-a，可理解为 annotated 的首字符，表示附注标签；-m 指定附注信息；直接给当前的提交版本创建一个附注标签） OR `git tag -a 标签名称 提交版本 -m 附注信息`（给指定的提交版本创建一个附注标签） |

两者的区别？？？

## 查看标签

- `git tag`
- `git tag --list`
- `git tag -l`
- `git tag -l xxxx`：根据 xxxx 进行标签的筛选，例如：`git tag -l "v4.0*"`（筛选标签进行查看：通配符*）

## 查看标签的提交信息

- `git show 标签名`：查看标签信息

## 删除标签

- `git tag -d 标签名称`：删除指定名称的标签

## 切换到标签分支

`git checkout 标签名称` 切换到指定 tag

## 拉取远程标签

- `git pull origin 标签名称`：拉取远程仓库指定标签到本地环境下当前标签

## 推送本地标签到远程仓库

- `git push origin --tags`：推送所有的标签到远程仓库
- `git push origin 标签名称`：推送指定标签到远程仓库

## 检出标签

检出标签：想在这个标签的基础上进行其他的开发操作；

操作实质 ： 就是以标签指定的版本为基础版本，新建一个分支，继续其他的操作，因此 ，就是 新建分支的操作了；

`git checkout -b 分支名称 标签名称`：以标签为基础，创建一个新的分支，在新的分支上进行开发

## 实例验证标签里代码是否可更改

使用自己在 Github 上已开发的一个项目 [custom-command-lsx](https://github.com/lushuixi/custom-command-lsx)（实现自定义命令sugar123），没有创建标签时的样子

![](E:\github\custom-command-lsx\gitTagImages\init-1.png)

当前有1个分支（main），0个标签

### 创建轻量标签

`git tag 1.0.0 `

![](E:\github\custom-command-lsx\gitTagImages\create-tag-1.png)

可以看到，我当前在 main 分支，创建了标签 1.0.0 后，并没有切换到该标签，还是在 main 分支

### 切换到标签

`git checkout 1.0.0 `

![](E:\github\custom-command-lsx\gitTagImages\checkout-tag-1.png)

从分支 main 切换到标签 1.0.0，打印了信息，来看下：

```txt
Note: switching to '1.0.0'. 

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.
// 当前处在独立的HEAD状态，你可以四处看看，可以更改文件并且提交更改，你也可以忽视这些 commits，切换分支不而不会影响任何分支的状态

If you want to create a new branch to retain commits you create, you may 
do so (now or later) by using -c with the switch command. Example:
// 如果你想创建一个新的分支来保存这些 commits，现在或后面可以使用 `switch -c` 命令

  git switch -c <new-branch-name>

Or undo this operation with（或者使用 `git switch -`，撤销此操作）:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false
// 通过将配置变量 advice.detachedHead 设置为 false 来关闭此建议

HEAD is now at a9d2313 完善readme // 指针位置
```

### make experimental changes

做一些修改，创建一个文件 gitTag.js：

![](E:\github\custom-command-lsx\gitTagImages\update-1.png)

看下，要是不提交，允不允许切换到 main 分支，答案是允许，但是这个更改会在你切换的分支里（在标签 1.0.0 **工作区**做出了更改，但是没有提交到**暂存区**，切换到分支 main，原标签 1.0.0 的更改就会跑到分支 main 上，还可以在分支 main 切换到标签 1.0.0，即 **工作区的更改不随分支切换而改变**）。

但是我在标签 1.0.0 **工作区的更改** 提交到暂存区（git add），并且提交到本地仓库（commit），看下变化，HEAD指针变化了 **8a66075**，看下我们当前的分支名称。

![](E:\github\custom-command-lsx\gitTagImages\commit-1.png)

即标签 1.0.0 工作区有更改，并提交到了本地仓库，好像生成了一个新的标签，此时我切换到分支 main

![](E:\github\custom-command-lsx\gitTagImages\checkout-tag-2.png)

发现，我前面的更改消失了，本地仓库也没有了即 **You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.**

### retain commits

如果我想要保存这些更改呢？试一下，更改同上

![](E:\github\custom-command-lsx\gitTagImages\udpate-2.png)

提交修改到本地仓库，当前标签 1.0.0 的 HEAD 指针更改到我刚 commit 的记录 a379337，但是我想保留这些更改，执行 `git switch -c test_git`，新建一个分支 test_git 保存当前的更改

![](E:\github\custom-command-lsx\gitTagImages\retain-changes-1.png)

此时，我们再切换到标签 1.0.0，会发现原更改已消失不见了，好像从来没有存在过一样，但是我们多了一个分支 test_git。

这便是 **If you want to create a new branch to retain commits you create, you may 
do so (now or later) by using -c with the switch command. Example: git switch -c <new-branch-name>**。

### undo this operation

撤销此操作怎么办？很奇怪，`git switch -` 切换到前面通过 switch 新建的分支 test_git 了

![](E:\github\custom-command-lsx\gitTagImages\undo-1.png)

在提交到本地仓库后，执行 `git switch -` 是撤销当前的 commit，回到上一个分支（切换到当前标签1.0.0的那个分支），从而实现 `undo this operation`。

### 总结

| 当前更改           | tag（1.0.0）                                                 | branch（main）                                               |
| ------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 工作区             | 切换到 main 分支，tag 1.0.0 工作区的更改内容会到 main 分支，tag 1.0.0 工作区的更改**消失** | 同 tag                                                       |
| 暂存区（add）      | 切换到 main 分支，tag 1.0.0 工作区的更改内容会到 main 分支，tag 1.0.0 工作区的更改**还在** | 同 tag                                                       |
| 本地仓库（commit） | 切换到 main 分支，tag 1.0.0 工作区的更改内容消失，main 分支也没有任何更改 | 切换到 tag 1.0.0 ，tag 1.0.0 工作区的更改内容消失，main 分支的更改内容已提交到本地仓库 |

### 推送标签到远程仓库

执行 `git push origin 1.0.0` 将本地标签 1.0.0 推送gt 到远程仓库，可以看到，有标签了

![](E:\github\custom-command-lsx\gitTagImages\push-tag-1.png)

## 线上标签更改

若是在标签上有更改呢？在实际开发中，可能要对线上问题处理，处理方式有二：

- 要么在标签上做出更改，并commit，通过 `git switch -c 分支名称`，新建一个分支保存标签上的更改，并重新新建标签和推送标签到远程仓库；
- OR 直接 `git checkout -b 分支名称 标签名称`，以标签为模板创建一个新的分支，然后在这个新的分支上更改，后面再新建标签和推送标签到远程仓库

![](E:\github\custom-command-lsx\gitTagImages\checkout-3.png)

## git checkout 和 git switch 区别

## 参考资料

- [Git基础 - git tag 一文真正的搞懂git标签的使用](https://blog.csdn.net/qq_39505245/article/details/124705850)