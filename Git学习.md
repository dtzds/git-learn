# Git学习：widows:

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

为什么Git添加文件需要`add`，`commit`一共两步呢？因为`commit`可以一次提交很多文件，所以你可以多次`add`不同的文件，比如：

```
$ git add file1.txt
$ git add file2.txt file3.txt
$ git commit -m "add 3 files."
```

