# Git学习：widows:

## 仓库创建和文件添加：

1、安装完成后设置用户名和邮箱

```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

2、创建版本库

先创建一个文件夹，然后再使用git init进行仓库创建

```
$ mkdir repository
$ cd repository
$ git init
Initialized empty Git repository in /Users/michael/learngit/.git/
```

**注：不要修改.git，这是Git用来跟踪管理版本库的**

3、添加文件到仓库中

1. 创建一个文件（**注：不要使用windows自带的文本编辑器**）并放到创建的仓库(repository)的目录下（或其子目录）

2. 使用`git add`将文件添加到仓库中：

   ```
   git add Git学习.md
   ```

3. 使用 `git commit`告诉Git，把文件提交到仓库：

   ```
   $ git commit -m "git学习"
   [master (root-commit) 3758339] git学习
    1 file changed, 23 insertions(+)
    create mode 100644 "Git\345\255\246\344\271\240.md" 
   ```

4. 为什么Git添加文件需要`add`，`commit`一共两步呢？因为`commit`可以一次提交很多文件，所以你可以多次`add`不同的文件，比如：

   ```
   $ git add file1.txt
   $ git add file2.txt file3.txt
   $ git commit -m "add 3 files."
   ```

## 文件修改：

1. 修改Git学习.md文件，再使用`git status`查看仓库当前状况：

   ```
   $ git status
   On branch master
   Changes not staged for commit:
     (use "git add <file>..." to update what will be committed)
     (use "git restore <file>..." to discard changes in working directory)
           modified:   "Git\345\255\246\344\271\240.md"
   
   no changes added to commit (use "git add" and/or "git commit -a")
   ```

   这说明文件已经被修改过了，但还没有提交`commit`的修改

2. 使用 git `diff` 查看文件修改的部分：

   ```
   $ git diff readme.txt 
   diff --git a/readme.txt b/readme.txt
   index 46d49bf..9247db6 100644
   --- a/readme.txt
   +++ b/readme.txt
   @@ -1,2 +1,2 @@			//git修改的部分
   -Git is a version control system.
   +Git is a distributed version control system.
    Git is free software.
   ```

3. 使用 `git add` 再进行一次提交，再使用 `git status` 查看状态

   ```
   $ git add Git学习.md
   $ git status
   On branch master
   Changes to be committed:
     (use "git restore --staged <file>..." to unstage)
           modified:   Git学习.md
   ```

    `git status`告诉我们，将要被提交的修改包括`readme.txt`，下一步，就可以放心地提交了： 

   ```
   $  git commit -m "添加了一些学习笔 ""
   [master 28e2f28] 添加了一些学习笔记“ git commit -m 添加了一些学习笔记
    1 file changed, 27 insertions(+), 1 deletion(-)
   ```

   再查看以下状态：

   ```
   $ git status
   On branch master
   nothing to commit, working tree clean
   ```

   没有什么东西需要提交了

## 版本回退

使用`git log` 进行查看版本日志

```
$ git log
warning: log.mailmap is not set; its implicit value will change in an
upcoming release. To squelch this message and preserve current
behaviour, set the log.mailmap configuration value to false.

To squelch this message and adopt the new behaviour now, set the
log.mailmap configuration value to true.

See 'git help config' and search for 'log.mailmap' for further information.

commit 9fd0fede4f311d07f2e281cdf9811eb864671f93 (HEAD -> master)
Author: dtzds <1356871485@qq.com>
Date:   Tue Mar 3 20:54:41 2020 +0800

add

commit 28e2f28f29a9e27aa34c2864d522584a49c77831
Author: dtzds <1356871485@qq.com>
Date:   Tue Mar 3 20:40:36 2020 +0800

添加了一些学习笔记“
git commit -m 添加了一些学习笔记

commit 3758339098eb68fd0f7bf81ada3fcd68a20f18cb
Author: dtzds <1356871485@qq.com>
Date:   Tue Mar 3 20:11:28 2020 +0800

git学习
```

我们可以看到有3个版本，每个commit后面的一串数字为`commit id` 即版本号

如果嫌弃输出的信息太多，可以加上--pretty=oneline参数

```
$ git log --pretty=oneline
9fd0fede4f311d07f2e281cdf9811eb864671f93 (HEAD -> master) add
28e2f28f29a9e27aa34c2864d522584a49c77831 添加了一些学习笔记“ git commit -m 添加
了一些学习笔记
3758339098eb68fd0f7bf81ada3fcd68a20f18cb git学习
```

其中`HEAD`表示当前版本 ，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`。 

将该文件回退到上一个版本：

```
$ git reset --hard HEAD^^
HEAD is now at 3758339 git学习
```

查看文件`Git.md`，发现文件的确回退到`git 学习`的版本，查看日志也没有后面的两个版本，该怎么回到未来的某个版本，只要当前窗口没有关闭，找到该版本号是可以回去的。

比如：回到`add`版本（只需要版本id的前几位）

```
$ git reset --hard 9fd0fede4f311d07f2e281cdf9811eb864671f93
HEAD is now at 9fd0fed add
```

git中其实就是将HEAD指针指向某个版本

**当你把版本回退到之前的版本后，又把电脑关了，想要把版本恢复回去该怎么做？**

当然我们要知道该版本的版本号，git提供了一个命令`git reflog`可以记录每次的命令

```
$ git reflog
warning: log.mailmap is not set; its implicit value will change in an
upcoming release. To squelch this message and preserve current
behaviour, set the log.mailmap configuration value to false.

To squelch this message and adopt the new behaviour now, set the
log.mailmap configuration value to true.

See 'git help config' and search for 'log.mailmap' for further information.

9fd0fed (HEAD -> master) HEAD@{0}: reset: moving to 9fd0fede4f311d07f2e281cdf9811eb864671f93
3758339 HEAD@{1}: reset: moving to HEAD^^
9fd0fed (HEAD -> master) HEAD@{2}: commit: add
28e2f28 HEAD@{3}: commit: 添加了一些学习笔记“
3758339 HEAD@{4}: commit (initial): git学习
```

我们可以得知`add` 版本的版本号为 `9fd0fed` 

## 暂存区和工作区

**工作区**即我们电脑里能看到的目录 

.git目录不算工作区，而是一个git版本库，版本库中有一个**`暂存区`** ，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。 

1、`git add` 就是把文件添加到暂存区中

2、`git commit` 则是将暂存区中的内容一次性提交到`master`分支中区

## 修改

**git是管理修改的而不是管理文件的**

### 撤销修改：

**场景1**：你修改了工作区中的文件，但还没有`git add`到暂存区中，该如何撤销？

我们使用`git status` 可以知道，`git restore <file>`可以将工作区中的修改回退回去

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   Git学习.md

no changes added to commit (use "git add" and/or "git commit -a")
```

故可以使用`git restore Git学习.md` 命令进行回退，git还提供了另一个命令`git checkout -- Git学习.md` 也可以进行回退

**场景二：**你修改了工作区中的文件，且已经`git add` 到暂存区中，该如何进行回退？

1、先将暂存区修改撤销掉，使用`git status`查看发现可以使用`git restore -- staged <file>`经暂存区回退到工作区

```
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   Git学习.md
```

故可以使用`git restore --staged Git学习.md` 回退，也可以使用`git reset HEAD Git学习.md` 进行撤销到工作区，即回到场景一

2、再重复场景一的操作就行

**场景三：** 已经将修改了的文件提交到版本库，不过不是提交到远程库

可以使用版本回退

```
git reset --hard commit_id
```

## 删除文件

1、在目录中手动删除了文件，使用`git status` 查看状态

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        deleted:    ddddd.md

no changes added to commit (use "git add" and/or "git commit -a")
```

提示可以使用`git add/rm <file>` 进行更新，即删除，或者使用`git restore <file>` 来进行恢复工作区文件

2、使用`git rm <file>` 删除文件的方式（目录中没有删除），使用`git status` 查看文件状态

```
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        deleted:    ddddd.md
```

发现可以使用`git restore -- staged <file>` 的方式进行回退到工作区中，由此可以联想到**撤销修改**的操作，可以回退回去，故工作区中的内容可以通过版本库来进行恢复，故只要提交了更新，就不怕误删文件

则删除文件的操作是：**

```
1、手动删除或rm文件
2、git add/rm test.md
3、git commit -m ""
```

或者是：`git rm <file>` -> `git commit`

## 远程仓库

### 添加远程仓库

1. **建立连接**

   首先在本地建立了一个仓库，在GitHub中创建了一个远程仓库，使两个仓库连接进行远程同步，GitHub上的仓库就可以作为备份使用，也可以让其他人使用其协作，假设GitHub仓库名为git-learn

   在GItHub中我们可以使用https也可以使用SSH

   1、使用`https`连接时：

   使用命令`$ git remote add origin https://github.com/dtzds/git-learn.git`来进行添加远程仓库

   2、使用`SSH`连接时：

   使用命令`$ git remote add origin git@github.com:dtzds/git-learn.git`来进行添加远程仓库

   其中`origin`是远程库的名字，也可以使用别的名字

2. **把内容推送到GitHub中去：**

   使用`git push -u origin master`把本地库的所有内容推送到远程库中：

   陶志胜@DESKTOP-BE8RFTA MINGW64 /repository (master)

   ```
   $ git push -u origin master
   fatal: TaskCanceledException encountered.
      ▒▒ȡ▒▒һ▒▒▒▒▒▒
   Enumerating objects: 5, done.
   Counting objects: 100% (5/5), done.
   Delta compression using up to 4 threads
   Compressing objects: 100% (2/2), done.
   Writing objects: 100% (3/3), 768 bytes | 768.00 KiB/s, done.
   Total 3 (delta 1), reused 0 (delta 0)
   remote: Resolving deltas: 100% (1/1), completed with 1 local object.
   To https://github.com/dtzds/git-learn.git
      cf598d9..c38ce71  master -> master
   Branch 'master' set up to track remote branch 'master' from 'origin'.
   ```

    加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令 

## 分支管理

在版本回退里，你已经知道，每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支。截止到目前，只有一条时间线，在Git里，这个分支叫主分支，即`master`分支。`HEAD`严格来说不是指向提交，而是指向`master`，`master`才是指向提交的，所以，`HEAD`指向的就是当前分支。

一开始的时候，`master`分支是一条线，Git用`master`指向最新的提交，再用`HEAD`指向`master`，就能确定当前分支，以及当前分支的提交点：

![git-br-initial](https://www.liaoxuefeng.com/files/attachments/919022325462368/0)

每次提交，`master`分支都会向前移动一步，这样，随着你不断提交，`master`分支的线也越来越长。

当我们创建新的分支，例如`dev`时，Git新建了一个指针叫`dev`，指向`master`相同的提交，再把`HEAD`指向`dev`，就表示当前分支在`dev`上：

![git-br-create](https://www.liaoxuefeng.com/files/attachments/919022363210080/l)

你看，Git创建一个分支很快，因为除了增加一个`dev`指针，改改`HEAD`的指向，工作区的文件都没有任何变化！

不过，从现在开始，对工作区的修改和提交就是针对`dev`分支了，比如新提交一次后，`dev`指针往前移动一步，而`master`指针不变：

![git-br-dev-fd](https://www.liaoxuefeng.com/files/attachments/919022387118368/l)

假如我们在`dev`上的工作完成了，就可以把`dev`合并到`master`上。Git怎么合并呢？最简单的方法，就是直接把`master`指向`dev`的当前提交，就完成了合并：

![git-br-ff-merge](https://www.liaoxuefeng.com/files/attachments/919022412005504/0)

所以Git合并分支也很快！就改改指针，工作区内容也不变！

合并完分支后，甚至可以删除`dev`分支。删除`dev`分支就是把`dev`指针给删掉，删掉后，我们就剩下了一条`master`分支：

![git-br-rm](https://www.liaoxuefeng.com/files/attachments/919022479428512/0)

真是太神奇了，你看得出来有些提交是通过分支完成的吗？

下面开始实战。

首先，我们创建`dev`分支，然后切换到`dev`分支：

```
$ git checkout -b dev
Switched to a new branch 'dev'
```

`git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令：

```
$ git branch dev			//创建分支
$ git checkout dev			//切换分支
Switched to branch 'dev'
```

切换分支建议使用`git switch dev`

使用`git branch`命令查看当前分支：

```
$ git branch				//查看分支
* dev
  master
```

`git branch`命令会列出所有的分支，当前分支前用`*`标识。

然后提交：

```
$ git add readme.txt 
$ git commit -m "branch test"
[dev b17d20e] branch test
 1 file changed, 1 insertion(+)
```

现在，`dev`分支的工作完成，我们就可以切换回`master`分支：

```
$ git checkout master
Switched to branch 'master'
```

切换回`master`分支后，再查看一个`readme.txt`文件，刚才添加的内容不见了！因为那个提交是在`dev`分支上，而`master`分支此刻的提交点并没有变：

![git-br-on-master](https://www.liaoxuefeng.com/files/attachments/919022533080576/0)

现在，我们把`dev`分支的工作成果合并到`master`分支上：

```
$ git merge dev				//合并分支到master上
Updating d46f35e..b17d20e
Fast-forward
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```

`git merge`命令用于合并指定分支到当前分支。合并后，再查看`readme.txt`的内容，就可以看到，和`dev`分支的最新提交是完全一样的。

 注意到上面的`Fast-forward`信息，Git告诉我们，这次合并是“快进模式”，也就是直接把`master`指向`dev`的当前提交，所以合并速度非常快。 

合并完成后，就可以放心地删除`dev`分支了：

```
$ git branch -d dev
Deleted branch dev (was b17d20e).
```

删除后，查看`branch`，就只剩下`master`分支了：

```
$ git branch
* master
```

因为创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在`master`分支上工作效果是一样的，但过程更安全。

**switch**

切换分支`git checkout <branch>`和撤销修改`git checkout -- <file>` ,一个命令有两个功能，故推荐使用`switch`

创建并切换到新的分支`dev`,可以使用：

```
$ git switch -c dev
```

切换到分支`master`:

```
$ git switch master
```

### 分支管理策略 

通常，合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

下面我们实战一下`--no-ff`方式的`git merge`：

首先，仍然创建并切换`dev`分支：

```
$ git switch -c dev
```

修改`Git学习.md` 文件，并提交一个新的`commit`:

```
$ git add Git学习.md
$ git commit -m "add merge"
[dev 7682a80] add merge
 1 file changed, 54 insertions(+), 4 deletions(-)
```

然后切换回`master`:

```
$ git switch master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)
```

并使用`--no-ff` 参数进行分支合并，表示禁用`Fast forward`:

```
$ git merge --no-ff -m "merge with no-ff" dev
Merge made by the 'recursive' strategy.
 Git学习.md | 58 ++++++++++++++++++++++++++++++++++++++++++++++++++++++----
 1 file changed, 54 insertions(+), 4 deletions(-)
```

因为本次合并要创建一个新的commit，所以加上`-m`参数，把commit描述写进去。

合并后，我们用`git log`看看分支历史：

```
$ git log --graph --pretty=oneline --abbrev-commit
*   cc851d5 (HEAD -> master) merge with no-ff
|\
| * 7682a80 (dev) add merge
|/
* 69b4482 添加分支管理
* 8e3646a (origin/master) test
* c38ce71 添加了远程仓库的内容
* cf598d9 git基础学习
* b062ea7 test
* b204faa  e enter the commit message for your changes. Lines starting
* cdc2f09 test
* 9441aa2 test01
* 1d968ee test1
* 9fad183 test
* 36f6d4c add some thing about Git
* 9fd0fed add
* 28e2f28 添加了一些学习笔记“ git commit -m 添加了一些学习笔记
* 3758339 git学习
```

可以看到，不使用`Fast forward`模式，merge后就像这样：

![git-no-ff-mode](https://www.liaoxuefeng.com/files/attachments/919023225142304/0)

#### 分支策略

在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；

你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了。

所以，团队合作的分支看起来就像这样：

![git-br-policy](https://www.liaoxuefeng.com/files/attachments/919023260793600/0)

