## 

| git 主要命令                | 含义                               |
| --------------------------- | ---------------------------------- |
| git commit --amend          | 修改提交                           |
| git branch -f main HEAD~    | 使main分支强制切换到HEAD的父版本   |
| git cherry-pick commitid    | 合入commitid到当前分支             |
| git branch bugFix           | 在当前HEAD新建bugFix分支           |
| git checkout -b bugFix HEAD | 在当前HEAD新建并切换到bugFix分支   |
| git rebase -i HEAD~4        | 重新合并提交HEAD~4内的分支         |
| git rebase main bugFix      | 把bugFix分支合并到main分支上       |
| git merge main              | 把main分支合并到当前分支上         |
| git tab v0 c1               | 在c1上创建v0标签                   |
| git describe c1             | 显示离c1最近的标签描述             |
| git reset HEAD~1            | 向上移动分支C0C1C2main*->C0C1main* |
| git revert HEAD             | 撤销 C0C1C2main*->C0C1C2C2'main*   |
| git branch bugWork main^^2^ |                                    |

## 

| git 远程命令 | 含义               |
| ------------ | ------------------ |
| git clone    |                    |
| git          |                    |
| git fetch    | 从远程仓库获取数据 |
|              |                    |

git 乱码：git bash中输入【git config --global core.quotepath false】或者在git的全局配置文件中，输入【quotepath = false】

当我们需要删除暂存区或分支上的文件，但是本地 ‘需要’ 这个文件，只是 ‘不希望加入版本控制’，可以使用 ‘git rm -r --cached’
git rm -r --cached target-dir

当我们需要删除暂存区或分支上的文件，同时工作区 ‘不需要’ 这个文件，可以使用 ‘git rm’
git rm file

# git忽略某个目录或文件不上传

1. 在目录中右键选择git bash Here

2. 然后输入touch .gitignore
3. 输入要忽略的文件或文件夹，如

```
.idea
.idea/*
```

# Push & Pull —— Git 远程仓库！

## git fetch

C0C1main*o/main -> C0C1C2C3main git fetch 后 C0C1mainC2C3o/main

### git fetch 做了些什么

`git fetch` 完成了仅有的但是很重要的两步:

- 从远程仓库下载本地仓库中缺失的提交记录
- 更新远程分支指针(如 `o/main`)

`git fetch` 实际上将本地仓库中的远程分支更新成了远程仓库相应分支最新的状态。

如果你还记得上一节课程中我们说过的，远程分支反映了远程仓库在你**最后一次与它通信时**的状态，`git fetch` 就是你与远程仓库通信的方式了！希望我说的够明白了，你已经了解 `git fetch` 与远程分支之间的关系了吧。

`git fetch` 通常通过互联网（使用 `http://` 或 `git://` 协议) 与远程仓库通信。

### git fetch 不会做的事

`git fetch` 并不会改变你本地仓库的状态。它不会更新你的 `main` 分支，也不会修改你磁盘上的文件。

理解这一点很重要，因为许多开发人员误以为执行了 `git fetch` 以后，他们本地仓库就与远程仓库同步了。它可能已经将进行这一操作所需的所有数据都下载了下来，但是**并没有**修改你本地的文件。我们在后面的课程中将会讲解能完成该操作的命令 :D

所以, 你可以将 `git fetch` 的理解为单纯的下载操作。

## Git Pull

既然我们已经知道了如何用 `git fetch` 获取远程的数据, 现在我们学习如何将这些变化更新到我们的工作当中。

其实有很多方法的 —— 当远程分支中有新的提交时，你可以像合并本地分支那样来合并远程分支。也就是说就是你可以执行以下命令:

- `git cherry-pick o/main`
- `git rebase o/main`
- `git merge o/main`
- 等等

实际上，由于先抓取更新再合并到本地分支这个流程很常用，因此 Git 提供了一个专门的命令来完成这两个操作。它就是我们要讲的 `git pull`

我们先来看看 `fetch`、`merge` 依次执行的效果

### git fetch; git merge o/main

我们用 `fetch` 下载了 `C3`, 然后通过 `git merge o/main` 合并了这一提交记录。现在我们的 `main` 分支包含了远程仓库中的更新（在本例中远程仓库名为 `origin`）

如果使用 `git pull` 呢?

### git pull

同样的结果！这清楚地说明了 `git pull` 就是 git fetch 和 git merge 的缩写！

## Git Push

OK，我们已经学过了如何从远程仓库获取更新并合并到本地的分支当中。这非常棒……但是我如何与大家分享**我的**成果呢？

嗯，上传自己分享内容与下载他人的分享刚好相反，那与 `git pull` 相反的命令是什么呢？`git push`！

`git push` 负责将**你的**变更上传到指定的远程仓库，并在远程仓库上合并你的新提交记录。一旦 `git push` 完成, 你的朋友们就可以从这个远程仓库下载你分享的成果了！

你可以将 `git push` 想象成发布你成果的命令。它有许多应用技巧，稍后我们会了解到，但是咱们还是先从基础的开始吧……

*注意 —— `git push` 不带任何参数时的行为与 Git 的一个名为 `push.default` 的配置有关。它的默认值取决于你正使用的 Git 的版本，但是在教程中我们使用的是 `upstream`。 这没什么太大的影响，但是在你的项目中进行推送之前，最好检查一下这个配置。*

## 偏离的工作

假设你周一克隆了一个仓库，然后开始研发某个新功能。到周五时，你新功能开发测试完毕，可以发布了。但是 —— 天啊！你的同事这周写了一堆代码，还改了许多你的功能中使用的 API，这些变动会导致你新开发的功能变得不可用。但是他们已经将那些提交推送到远程仓库了，因此你的工作就变成了基于项目**旧版**的代码，与远程仓库最新的代码不匹配了。

这种情况下, `git push` 就不知道该如何操作了。如果你执行 `git push`，Git 应该让远程仓库回到星期一那天的状态吗？还是直接在新代码的基础上添加你的代码，亦或由于你的提交已经过时而直接忽略你的提交？

因为这情况（历史偏离）有许多的不确定性，Git 是不会允许你 `push` 变更的。实际上它会强制你先合并远程最新的代码，然后才能分享你的工作。

那该如何解决这个问题呢？很简单，你需要做的就是使你的工作基于最新的远程分支。

有许多方法做到这一点呢，不过最直接的方法就是通过 rebase 调整你的工作。咱们继续，看看怎么 rebase！

如果我们在 push 之前做 rebase 呢？

```
git fetch; git rebase o/main; git push
```

我们用 `git fetch` 更新了本地仓库中的远程分支，然后用 rebase 将我们的工作移动到最新的提交记录下，最后再用 `git push` 推送到远程仓库。

还有其它的方法可以在远程仓库变更了以后更新我的工作吗? 当然有，我们还可以使用 `merge`

尽管 `git merge` 不会移动你的工作（它会创建新的合并提交），但是它会告诉 Git 你已经合并了远程仓库的所有变更。这是因为远程分支现在是你本地分支的祖先，也就是说你的提交已经包含了远程分支的所有变化。

看下演示...

咱们们用 merge 替换 rebase 来试一下……

```
git fetch; git merge o/main; git push
```

我们用 `git fetch` 更新了本地仓库中的远程分支，然后**合并**了新变更到我们的本地分支（为了包含远程仓库的变更），最后我们用 `git push` 把工作推送到远程仓库

很好！但是要敲那么多命令，有没有更简单一点的？

当然 —— 前面已经介绍过 `git pull` 就是 fetch 和 merge 的简写，类似的 `git pull --rebase` 就是 fetch 和 rebase 的简写！

让我们看看简写命令是如何工作的。

这次用 `--rebase`……

```
git pull --rebase; git push
```

跟之前结果一样，但是命令更短了。

## 远程服务器拒绝!(Remote Rejected)

如果你是在一个大的合作团队中工作, 很可能是main被锁定了, 需要一些Pull Request流程来合并修改。如果你直接提交(commit)到本地main, 然后试图推送(push)修改, 你将会收到这样类似的信息:

```
! [远程服务器拒绝] main -> main (TF402455: 不允许推送(push)这个分支; 你必须使用pull request来更新这个分支.)
```

### 为什么会被拒绝?

远程服务器拒绝直接推送(push)提交到main, 因为策略配置要求 pull requests 来提交更新.

你应该按照流程,新建一个分支, 推送(push)这个分支并申请pull request,但是你忘记并直接提交给了main.现在你卡住并且无法推送你的更新.

新建一个分支feature, 推送到远程服务器. 然后reset你的main分支和远程服务器保持一致, 否则下次你pull并且他人的提交和你冲突的时候就会有问题.

# 关于 origin 和它的周边 —— Git 远程仓库高级操作

## 合并特性分支

既然你应该很熟悉 fetch、pull、push 了，现在我们要通过一个新的工作流来测试你的这些技能。

在大型项目中开发人员通常会在（从 `main` 上分出来的）特性分支上工作，工作完成后只做一次集成。这跟前面课程的描述很相像（把 side 分支推送到远程仓库），不过本节我们会深入一些.

但是有些开发人员只在 main 上做 push、pull —— 这样的话 main 总是最新的，始终与远程分支 (o/main) 保持一致。

对于接下来这个工作流，我们集成了两个步骤：

- 将特性分支集成到 `main` 上
- 推送并更新远程分支

让我们看看如何快速的更新 `main` 分支并推送到远程。

```
git pull --rebase; git push
```

我们执行了两个命令:

- 将我们的工作 rebase 到远程分支的最新提交记录
- 向远程仓库推送我们的工作

这个关卡的 Boss 很厉害 —— 以下是通关提示：

- 这里共有三个特性分支 —— `side1` `side2` 和 `side3`
- 我需要将这三分支按顺序推送到远程仓库
- 因为远程仓库已经被更新过了，所以我们还要把那些工作合并过来

:O 紧张了？祝你好运！完成了本关, 你就向目标又迈近了一大步啦！

## 为什么不用 merge 呢?

为了 push 新变更到远程仓库，你要做的就是**包含**远程仓库中最新变更。意思就是只要你的本地分支包含了远程分支（如 `o/main`）中的最新变更就可以了，至于具体是用 rebase 还是 merge，并没有限制。

那么既然没有规定限制，为何前面几节都在着重于 rebase 呢？为什么在操作远程分支时不喜欢用 `merge` 呢？

在开发社区里，有许多关于 merge 与 rebase 的讨论。以下是关于 rebase 的优缺点：

优点:

- Rebase 使你的提交树变得很干净, 所有的提交都在一条线上

缺点:

- Rebase 修改了提交树的历史

比如, 提交 C1 可以被 rebase 到 C3 之后。这看起来 C1 中的工作是在 C3 之后进行的，但实际上是在 C3 之前。

一些开发人员喜欢保留提交历史，因此更偏爱 merge。而其他人（比如我自己）可能更喜欢干净的提交树，于是偏爱 rebase。仁者见仁，智者见智。 :D

## 远程跟踪分支

在前几节课程中有件事儿挺神奇的，Git 好像知道 `main` 与 `o/main` 是相关的。当然这些分支的名字是相似的，可能会让你觉得是依此将远程分支 main 和本地的 main 分支进行了关联。这种关联在以下两种情况下可以清楚地得到展示：

- pull 操作时, 提交记录会被先下载到 o/main 上，之后再合并到本地的 main 分支。隐含的合并目标由这个关联确定的。
- push 操作时, 我们把工作从 `main` 推到远程仓库中的 `main` 分支(同时会更新远程分支 `o/main`) 。这个推送的目的地也是由这种关联确定的！

### 远程跟踪

直接了当地讲，`main` 和 `o/main` 的关联关系就是由分支的“remote tracking”属性决定的。`main` 被设定为跟踪 `o/main`—— 这意味着为 `main` 分支指定了推送的目的地以及拉取后合并的目标。

你可能想知道 `main` 分支上这个属性是怎么被设定的，你并没有用任何命令指定过这个属性呀！好吧, 当你克隆仓库的时候, Git 就自动帮你把这个属性设置好了。

当你克隆时, Git 会为远程仓库中的每个分支在本地仓库中创建一个远程分支（比如 `o/main`）。然后再创建一个跟踪远程仓库中活动分支的本地分支，默认情况下这个本地分支会被命名为 `main`。

克隆完成后，你会得到一个本地分支（如果没有这个本地分支的话，你的目录就是“空白”的），但是可以查看远程仓库中所有的分支（如果你好奇心很强的话）。这样做对于本地仓库和远程仓库来说，都是最佳选择。

这也解释了为什么会在克隆的时候会看到下面的输出：

```
local branch "main" set to track remote branch "o/main"
```

### 我能自己指定这个属性吗？

当然可以啦！你可以让任意分支跟踪 `o/main`, 然后该分支会像 `main` 分支一样得到隐含的 push 目的地以及 merge 的目标。 这意味着你可以在分支 `totallyNotMain` 上执行 `git push`，将工作推送到远程仓库的 `main` 分支上。

有两种方法设置这个属性，第一种就是通过远程分支检出一个新的分支，执行:

```
git checkout -b totallyNotMain o/main
```

就可以创建一个名为 `totallyNotMain` 的分支，它跟踪远程分支 `o/main`。

闲话少说，咱们先看看演示！我们检出一个名叫 `foo` 的新分支，让其跟踪远程仓库中的 `main`

```
git checkout -b foo o/main; git pull
```

正如你所看到的, 我们使用了隐含的目标 `o/main` 来更新 `foo` 分支。需要注意的是 main 并未被更新！

git push 同样适用

```
git checkout -b foo o/main; git commit; git push
```

我们将一个并不叫 `main` 的分支上的工作推送到了远程仓库中的 `main` 分支上

### 第二种方法

另一种设置远程追踪分支的方法就是使用：`git branch -u` 命令，执行：

```
git branch -u o/main foo
```

这样 `foo` 就会跟踪 `o/main` 了。如果当前就在 foo 分支上, 还可以省略 foo：

```
git branch -u o/main
```

看看这种方式的实际的效果...

```
git branch -u o/main foo; git commit; git push
```

跟之前一样, 但这个命令更明确！

## Git Push 的参数

很好! 既然你知道了远程跟踪分支，我们可以开始揭开 git push、fetch 和 pull 的神秘面纱了。我们会逐个介绍这几个命令，它们在理念上是非常相似的。

首先来看 `git push`。在远程跟踪课程中，你已经学到了 Git 是通过当前检出分支的属性来确定远程仓库以及要 push 的目的地的。这是未指定参数时的行为，我们可以为 push 指定参数，语法是：

```git
git push <remote> <place>
```

`<place>` 参数是什么意思呢？我们稍后会深入其中的细节, 先看看例子, 这个命令是:

```
git push origin main
```

把这个命令翻译过来就是：

*切到本地仓库中的“main”分支，获取所有的提交，再到远程仓库“origin”中找到“main”分支，将远程仓库中没有的提交记录都添加上去，搞定之后告诉我。*

我们通过“place”参数来告诉 Git 提交记录来自于 main, 要推送到远程仓库中的 main。它实际就是要同步的两个仓库的位置。

需要注意的是，因为我们通过指定参数告诉了 Git 所有它需要的信息, 所以它就忽略了我们所检出的分支的属性！

我们看看指定参数的例子。注意下我们当前检出的位置。

```
git checkout C0; git push origin main
```

好了! 通过指定参数, 远程仓库中的 `main` 分支得到了更新。

如果不指定参数会发生什么呢?

```
git checkout C0; git push
```

命令失败了（正如你看到的，什么也没有发生）! 因为我们所检出的 HEAD 没有跟踪任何分支。

### `<place>`参数详解

还记得之前课程说的吧，当为 git push 指定 place 参数为 `main` 时，我们同时指定了提交记录的来源和去向。

你可能想问 —— 如果来源和去向分支的名称不同呢？比如你想把本地的 `foo` 分支推送到远程仓库中的 `bar` 分支。

哎，很遗憾 Git 做不到…… 开个玩笑，别当真！当然是可以的啦 :) Git 拥有超强的灵活性（有点过于灵活了）

接下来咱们看看是怎么做的……

要同时为源和目的地指定 `<place>` 的话，只需要用冒号 `:` 将二者连起来就可以了：

```
git push origin <source>:<destination>
```

这个参数实际的值是个 refspec，“refspec” 是一个自造的词，意思是 Git 能识别的位置（比如分支 `foo` 或者 `HEAD~1`）

一旦你指定了独立的来源和目的地，就可以组织出言简意赅的远程操作命令了，让我们看看演示！

记住，`source` 可以是任何 Git 能识别的位置：

```
git push origin foo^:main
```

这是个令人困惑的命令，但是它确实是可以运行的 —— Git 将 `foo^` 解析为一个位置，上传所有未被包含到远程仓库里 `main` 分支中的提交记录。

如果你要推送到的目的分支不存在会怎么样呢？没问题！Git 会在远程仓库中根据你提供的名称帮你创建这个分支！

```
git push origin main:newBranch
```

很赞吧！它是不是很聪明？！ :D

## Git fetch 的参数

我们刚学习了 git push 的参数，很酷的 `<place>` 参数，还有用冒号分隔的 refspecs（`<source>:<destination>`）。 这些参数可以用于 `git fetch` 吗？

你猜中了！`git fetch` 的参数和 `git push` 极其相似。他们的概念是相同的，只是方向相反罢了（因为现在你是下载，而非上传）

让我们逐个讨论下这些概念……

### `<place>` 参数

如果你像如下命令这样为 git fetch 设置 的话：

```
git fetch origin foo
```

Git 会到远程仓库的 `foo` 分支上，然后获取所有本地不存在的提交，放到本地的 `o/foo` 上。

来看个例子（还是前面的例子，只是命令不同了）

通过指定 place...

```
git fetch origin foo
```

我们只下载了远程仓库中 `foo` 分支中的最新提交记录，并更新了 o/foo

你可能会好奇 —— 为何 Git 会将新提交放到 `o/foo` 而不是放到我本地的 foo 分支呢？之前不是说这样的 参数就是同时应用于本地和远程的位置吗？

好吧, 本例中 Git 做了一些特殊处理，因为你可能在 foo 分支上的工作还未完成，你也不想弄乱它。还记得在 `git fetch`课程里我们讲到的吗 —— 它不会更新你的本地的非远程分支, 只是下载提交记录（这样, 你就可以对远程分支进行检查或者合并了）。

“如果我们指定 `<source>:<destination>` 会发生什么呢？”

如果你觉得直接更新本地分支很爽，那你就用冒号分隔的 refspec 吧。不过，你不能在当前检出的分支上干这个事，但是其它分支是可以的。

这里有一点是需要注意的 —— `source` 现在指的是远程仓库中的位置，而 `<destination>` 才是要放置提交的本地仓库的位置。它与 git push 刚好相反，这是可以讲的通的，因为我们在往相反的方向传送数据。

理论上虽然行的通，但开发人员很少这么做。我在这里介绍它主要是为了从概念上说明 `fetch` 和 `push` 的相似性，只是方向相反罢了。

来看个疯狂的例子：

```
git fetch origin foo~1:bar
```

哇! 看见了吧, Git 将 `foo~1` 解析成一个 origin 仓库的位置，然后将那些提交记录下载到了本地的 `bar` 分支（一个本地分支）上。注意由于我们指定了目标分支，`foo` 和 `o/foo` 都没有被更新。

如果执行命令前目标分支不存在会怎样呢？我们看一下上个对话框中没有 bar 分支的情况。

```
git fetch origin foo~1:bar
```

看见了吧，跟 git push 一样，Git 会在 fetch 前自己创建立本地分支, 就像是 Git 在 push 时，如果远程仓库中不存在目标分支，会自己在建立一样。

没有参数呢?

如果 `git fetch` 没有参数，它会下载所有的提交记录到各个远程分支……

git fetch

相当简单，但是仅需更新一次，值得你去做！

## 古怪的 `<source>`

Git 有两种关于 `<source>` 的用法是比较诡异的，即你可以在 git push 或 git fetch 时不指定任何 `source`，方法就是仅保留冒号和 destination 部分，source 部分留空。

- `git push origin :side`
- `git fetch origin :bugFix`

我们分别来看一下这两条命令的作用……

如果 push 空 到远程仓库会如何呢？它会删除远程仓库中的分支！

```
git push origin :foo
```

就是这样子, 我们通过给 push 传空值 source，成功删除了远程仓库中的 `foo` 分支, 这真有意思...

如果 fetch 空 到本地，会在本地创建一个新分支。

```
git fetch origin :bar
```

很神奇吧！但无论怎么说, 这就是 Git！

## Git pull 参数

既然你已经掌握关于 `git fetch` 和 `git push` 参数的方方面面了，关于 git pull 几乎没有什么可以讲的了 :)

因为 git pull 到头来就是 fetch 后跟 merge 的缩写。你可以理解为用同样的参数执行 git fetch，然后再 merge 你所抓取到的提交记录。

还可以和其它更复杂的参数一起使用, 来看一些例子:

以下命令在 Git 中是等效的:

`git pull origin foo` 相当于：

```
git fetch origin foo; git merge o/foo
```

还有...

`git pull origin bar~1:bugFix` 相当于：

```
git fetch origin bar~1:bugFix; git merge bugFix
```

看到了? git pull 实际上就是 fetch + merge 的缩写, git pull 唯一关注的是提交最终合并到哪里（也就是为 git fetch 所提供的 destination 参数）

一起来看个例子吧：

如果我们指定要抓取的 place，所有的事情都会跟之前一样发生，只是增加了 merge 操作

```
git pull origin main
```

看到了吧! 通过指定 `main` 我们更新了 `o/main`。然后将 `o/main` merge 到我们的检出位置，**无论**我们当前检出的位置是哪。

pull 也可以用 source:destination 吗? 当然喽, 看看吧:

```
git pull origin main:foo
```

哇, 这个命令做的事情真多。它先在本地创建了一个叫 `foo`的分支，从远程仓库中的 main 分支中下载提交记录，并合并到 `foo`，然后再 merge 到我们的当前检出的分支 `bar` 上。操作够多的吧？！



show solution