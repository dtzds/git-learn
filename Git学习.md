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



