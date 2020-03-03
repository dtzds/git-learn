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

