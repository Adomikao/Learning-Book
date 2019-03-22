# 集中式 & 分布式

Git 属于分布式版本控制系统，而 SVN 属于集中式版本控制器。

集中式版本控制系统最大的缺点就是必须联网才能工作，如果在局域网还好，可如果在互联网，遇到网速慢，提交文件会很慢。

这里有几个概念：本地、服务器、中央服务器（远程服务器）。每一次commit是提交到本本机的服务器，这个不需要联网，正所谓的版本管理，就是要方便我们知道每一个版本，比如回到之前的某个版本（这是其一），而且回退到某个之前的版
本，也是从本机的服务器拿的数据，这些都不需要联网。而 SVN 的每一次 commit 都需要联网，这就需要网络的等待。 Git只有在Push、pull 的时候需要联网，而我们平时更多的操作应是commit。

对于svn必须联网这一点，我们都是在一个办公室，所以用的都是局域网，速度可以接受，对于远程开发同步代码的情况比较少，暂时体会不深。

分布式版本控制系统根本没有 `中央服务器` ，每台电脑上都有都是一个完整的版本库，这样工作就不需要联网，因为版本库在电脑上。

分布式版本控制系统更加安全，因为每个人都有完整的版本库。而集中式版本控制系统的中央服务器出了问题，则所有人都无法干活。


# 创建版本库
版本库，又名仓库，英文名 `repository` 。可以理解成一个简单的目录，里面所有文件都可以被 git 管理起来。Git 能跟踪每个文件的添加，修改，和删除，以便追踪历史或者还原。

创建一个版本库，以选择一个合适的位置，创建一个空目录,使用 `git init` 命令初始化仓库。目录下面会产生一个隐藏的 `.git` 目录。
```sh
$ mkdir sakura
$ cd sakura
$ git init
Initialized empty Git repository in D:/code/other/sakura/.git/
```
我们创建一个普通文本 `readme.txt` ,在里面编写任意文字，使用 `git add` 命令把文件添加到仓库中，然后使用 `git commit` 命令进行提交操作。其中 `-m` 后面是本次提交的说明，
```sh
$ git add readme.txt
$ git commit -m "create readme.txt file"
[master (root-commit) c2548e2] create readme.txt file
1 file changed, 1 insertion(+)
create mode 100644 readme.txt
```
> Git 提交文件需要 add ，commit 一共两步，因为 commit 可以一次提交很多文件，所以你可以多次 add 不同的文件。

# 文件操作
我们用编辑器继续修改 `readme.txt` 文件，把 `sakura is nice.` 改成 `sakura is beautiful.` , 使用 `git status` 命令查看仓库当前状态。输出告诉我们 `readme.txt` 被修改了，但没有提交修改。
```sh
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```
使用 `git diff` 命令查看 difference 。
```
$ git diff readme.txt
diff --git a/readme.txt b/readme.txt
index de95d80..9b80c4f 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1 +1 @@
-sakura is nice.
+sakura is beautiful.
\ No newline at end of file
```
使用 `git add` , `git commit` 提交文件后，再查看仓库的当前状态。
```
$ git status
On branch master
nothing to commit, working tree clean
```

