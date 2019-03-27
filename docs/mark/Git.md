# 集中式 & 分布式

Git 属于分布式版本控制系统，而 SVN 属于集中式版本控制器。

集中式版本控制系统最大的缺点就是必须联网才能工作，如果在局域网还好，可如果在互联网，遇到网速慢，提交文件会很慢。

这里有几个概念：本地、服务器、中央服务器（远程服务器）。每一次 `commit` 是提交到本本机的服务器，这个不需要联网，正所谓的版本管理，就是要方便我们知道每一个版本，比如回到之前的某个版本（这是其一），而且回退到某个之前的版
本，也是从本机的服务器拿的数据，这些都不需要联网。而 SVN 的每一次 `commit` 都需要联网，这就需要网络的等待。 Git 只有在 `Push` 、`Pull` 的时候需要联网，而我们平时更多的操作应是`commit`。

对于 SVN 必须联网这一点，我们都是在一个办公室，所以用的都是局域网，速度可以接受，对于远程开发同步代码的情况比较少，暂时体会不深。

分布式版本控制系统根本没有 `中央服务器` ，每台电脑上都有都是一个完整的版本库，这样工作就不需要联网，因为版本库在电脑上。

分布式版本控制系统更加安全，因为每个人都有完整的版本库。而集中式版本控制系统的中央服务器出了问题，则所有人都无法干活。


# 创建版本库
版本库，又名仓库，英文名 `repository` 。可以理解成一个简单的目录，里面所有文件都可以被 git 管理起来。Git 能跟踪每个文件的添加，修改，和删除，以便追踪历史或者还原。

创建一个版本库，以选择一个合适的位置，创建一个空目录,使用 `git init` 命令初始化仓库。目录下面会产生一个隐藏的 `.git` 目录。
```
$ mkdir sakura
$ cd sakura
$ git init
Initialized empty Git repository in D:/code/other/sakura/.git/
```
我们创建一个普通文本 `readme.txt` ,在里面编写任意文字，使用 `git add` 命令把文件添加到仓库中，然后使用 `git commit` 命令进行提交操作。其中 `-m` 后面是本次提交的说明，
```
$ git add readme.txt
$ git commit -m "create readme.txt file"
[master (root-commit) c2548e2] create readme.txt file
1 file changed, 1 insertion(+)
create mode 100644 readme.txt
```
> Git 提交文件需要 add ，commit 一共两步，因为 commit 可以一次提交很多文件，所以你可以多次 add 不同的文件。

# 修改文件
我们用编辑器继续修改 `readme.txt` 文件，把 `sakura is nice.` 改成 `sakura is beautiful.` , 使用 `git status` 命令查看仓库当前状态。输出告诉我们 `readme.txt` 被修改了，但没有提交修改。
```
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

# 版本回退
在 Git 中，`HEAD` 表示当前版本，上一个版本用 `HEAD^` 表示，上上一个版本是 `HEAD^^` ，往上 100 个版本可以用 `HEAD~100` 。使用 `git reset` 命令回到上一个版本：

```
$ git reset --hard HEAD^
HEAD is now at c2548e2 create readme.txt file
```
使用 ` git reset --hard $commitId` 到指定版本号

```
$ git reset --hard 1094a
HEAD is now at 83b0afe append GPL
```
使用  `git reflog` 查看你的每一次命令：

```
$ git reflog
071f7f3 (HEAD -> master) HEAD@{0}: reset: moving to 071f
c2548e2 HEAD@{1}: reset: moving to HEAD^
071f7f3 (HEAD -> master) HEAD@{2}: commit: modified
c2548e2 HEAD@{3}: commit (initial): create readme.txt file
```
# 工作区 & 暂存区
工作区（Working Directory） 是指电脑上的工作文件夹， 如 `sakura`，工作区有一个隐藏目录 `.git` ，这个不算工作区，是 Git 的版本库，里面存了很多东西，最重要的是叫 stage （或 index）的暂存区，和 Git 为我们自动创建的第一个分支 `master` ，以及指向 `master` 的 `HEAD` 指针。

<div align="center"> <img src="pics/trees.png" width="550" > </div><br>

本地仓库由 Git 维护的三棵 “树” 组成。第一个是 `工作目录`，持有实际文件；第二个是`暂存区（index）`，它像个缓存区域，临时保存你的修改；最后是 `HEAD` ，它指向最后一次保存的结果。

# 推送
现在改动已经在本地仓库的 `HEAD` 中了。使用 `git push origin master` 命令以将这些改动提交到远端仓库,可以把 `master` 换成你想要推送的任何分支。也可以使用 
`git remote add origin <server>` 命令将改动推送到所添加的服务器上去。

# 分支管理
在创建仓库的时候，`master` 是默认分支，在其他分支上进行开发，完成后再将它合并到主分支，分支是用来将特性开发绝缘开来的。
<div align="center"> <img src="pics/branches.png" width="550" > </div><br>

我们创建 `feature_x` 分支，然后切换到 `feature_x` 分支

```
$ git checkout -b feature_x
Switched to a new branch 'feature_x'
```
`git checkout` 命令加上 `-b` 参数表示创建并切换，相当于两条命令

```
$ git branch feature_x
$ git checkout feature_x
Switched to branch 'feature_x'
```
使用 `git branch` 命令查看当前分支

```
$ git branch
* feature_x
  master
```
现在可以在 feature_x 分支上进行开发，然后提交。工作完成我们就切换回 master 分支, 然后使用 `git merge` 命令吧 feature_x 分支上的工作成果合并到主分支。

```
$ git checkout master
Switched to branch 'master'
$ git merge feature_x
Updating c89e25h..a37c51a
Fast-forward
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```
合并完成后，可以使用 `git branch -d <name>` 删除 feature_x 分支

```
$ git branch -d feature_x
Deleted branch dev (was 071f7f3).
```
# .gitignore 文件
在 Git 工作区的目录有个特殊文件 .gitignore ，忽略特殊文件。忽略文件的原则是：
- 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件;
- 忽略编译生成的中间文件、可执行文件等;
- 忽略操作系统自动生成的文件，比如缩略图等。

# Git 常用命令
<div align="center"> <img src="pics/git-command.jpg" width="700" > </div><br>

详情见：http://www.cheat-sheets.org/saved-copy/git-cheat-sheet.pdf


# 参考资料
- [Learn Git Branching](https://learngitbranching.js.org/)
- [Git - the simple guid](http://rogerdudler.github.io/git-guide/index.html)
- [廖雪峰 : Git 教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
- [A Visual Git Reference](http://marklodato.github.io/visual-git-guide/index-en.html)
- [Git - Reference](https://www.git-scm.com/docs)


















