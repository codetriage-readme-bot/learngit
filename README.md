#Git教程

#1 Git简介
##Git的诞生
##集中式vs分布式

#2 安装Git
##在Windows上安装Git

	$ git config --global user.name "Your Name"
	$ git config --global user.email "email@example.com"

#3 创建版本库
什么是版本库呢？版本库又名仓库，英文名 **repository**，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

所以，创建一个版本库非常简单，首先，选择一个合适的地方，创建一个空目录：

	$ mkdir learngit
	$ cd learngit
	$ pwd
	/Users/michael/learngit

**pwd** 命令用于显示当前目录。

通过 **git init** 命令把这个目录变成Git可以管理的仓库：

	$ git init
	Initialized empty Git repository in /Users/michael/learngit/.git/

如果你没有看到**.git**目录，那是因为这个目录默认是隐藏的，用**ls -ah**命令就可以看见。

##把文件添加到版本库

readme.txt:

	Git is a version control system.
	Git is free software.

一定要放到 **learngit** 目录下（子目录也行），因为这是一个Git仓库，放到其他地方Git再厉害也找不到这个文件。

把一个文件放到Git仓库只需要两步。

第一步，用命令 **git add** 告诉Git，把文件添加到仓库：

	$ git add readme.txt

执行上面的命令，没有任何显示，这就对了，Unix的哲学是“没有消息就是好消息”，说明添加成功。

第二步，用命令 **git commit** 告诉Git，把文件提交到仓库：
	
	$ git commit -m "wrote a readme file"
	[master (root-commit) cb926e7] wrote a readme file
	 1 file changed, 2 insertions(+)
	 create mode 100644 readme.txt

简单解释一下 **git commit **命令，**-m**后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。

**git commit** 命令执行成功后会告诉你，1个文件被改动（我们新添加的readme.txt文件），插入了两行内容（readme.txt有两行内容）。

为什么Git添加文件需要 **add**，**commit** 一共两步呢？**因为commit** 可以一次提交很多文件，所以你可以多次 **add** 不同的文件，比如：

	$ git add file1.txt
	$ git add file2.txt file3.txt
	$ git commit -m "add 3 files."

##小结

现在总结一下今天学的两点内容：

初始化一个Git仓库，使用 **git init** 命令。

添加文件到Git仓库，分两步：

- 第一步，使用命令 **git add <file>**，注意，可反复多次使用，添加多个文件；
- 第二步，使用命令 **git commit**，完成。

#4 时光机穿梭

我们已经成功地添加并提交了一个readme.txt文件，现在，是时候继续工作了，于是，我们继续修改readme.txt文件，改成如下内容：

	Git is a distributed version control system.
	Git is free software.

现在，运行 **git status** 命令看看结果：

	$ git status
	# On branch master
	# Changes not staged for commit:
	#   (use "git add <file>..." to update what will be committed)
	#   (use "git checkout -- <file>..." to discard changes in working directory)
	#
	#    modified:   readme.txt
	#
	no changes added to commit (use "git add" and/or "git commit -a")

**git status** 命令可以让我们时刻掌握仓库当前的状态，上面的命令告诉我们，readme.txt被修改过了，但还没有准备提交的修改。

虽然Git告诉我们readme.txt被修改了，但如果能看看具体修改了什么内容，自然是很好的。比如你休假两周从国外回来，第一天上班时，已经记不清上次怎么修改的readme.txt，所以，需要用 **git diff** 这个命令看看：

	$ git diff readme.txt 
	diff --git a/readme.txt b/readme.txt
	index 46d49bf..9247db6 100644
	--- a/readme.txt
	+++ b/readme.txt
	@@ -1,2 +1,2 @@
	-Git is a version control system.
	+Git is a distributed version control system.
	 Git is free software.

**git diff** 顾名思义就是查看difference，显示的格式正是Unix通用的diff格式，可以从上面的命令输出看到，我们在第一行添加了一个“distributed”单词。

知道了对readme.txt作了什么修改后，再把它提交到仓库就放心多了，提交修改和提交新文件是一样的两步，第一步是 **git add**：

	$ git add readme.txt

同样没有任何输出。在执行第二步 **git commit** 之前，我们再运行**git status** 看看当前仓库的状态：

	$ git commit -m "add distributed"
	[master ea34578] add distributed
 	1 file changed, 1 insertion(+), 1 deletion(-)

提交后，我们再用 **git status** 命令看看仓库的当前状态：

	$ git status
	# On branch master
	nothing to commit (working directory clean)

Git告诉我们当前没有需要提交的修改，而且，工作目录是干净（working directory clean）的。

##小结

要随时掌握工作区的状态，使用 **git status** 命令。

如果 **git status** 告诉你有文件被修改过，用 **git diff** 可以查看修改内容。

##版本回退
现在，你已经学会了修改文件，然后把修改提交到Git版本库，现在，再练习一次，修改readme.txt文件如下：

	Git is a distributed version control system.
	Git is free software distributed under the GPL.

然后尝试提交：

	$ git add readme.txt
	$ git commit -m "append GPL"
	[master 3628164] append GPL
 	1 file changed, 1 insertion(+), 1 deletion(-)

像这样，你不断对文件进行修改，然后不断提交修改到版本库里，就好比玩RPG游戏时，每通过一关就会自动把游戏状态存盘，如果某一关没过去，你还可以选择读取前一关的状态。有些时候，在打Boss之前，你会手动存盘，以便万一打Boss失败了，可以从最近的地方重新开始。Git也是一样，每当你觉得文件修改到一定程度的时候，就可以“保存一个快照”，这个快照在Git中被称为 **commit**。一旦你把文件改乱了，或者误删了文件，还可以从最近的一个 **commit** 恢复，然后继续工作，而不是把几个月的工作成果全部丢失。

现在，我们回顾一下readme.txt文件一共有几个版本被提交到Git仓库里了：

版本1：wrote a readme file

	Git is a version control system.
	Git is free software.

版本2：add distributed

	Git is a distributed version control system.
	Git is free software.

版本3：append GPL

	Git is a distributed version control system.
	Git is free software distributed under the GPL.

当然了，在实际工作中，我们脑子里怎么可能记得一个几千行的文件每次都改了什么内容，不然要版本控制系统干什么。版本控制系统肯定有某个命令可以告诉我们历史记录，在Git中，我们用 **git log** 命令查看：

	$ git log
	commit 3628164fb26d48395383f8f31179f24e0882e1e0
	Author: Michael Liao <askxuefeng@gmail.com>
	Date:   Tue Aug 20 15:11:49 2013 +0800
	
	    append GPL
	
	commit ea34578d5496d7dd233c827ed32a8cd576c5ee85
	Author: Michael Liao <askxuefeng@gmail.com>
	Date:   Tue Aug 20 14:53:12 2013 +0800
	
	    add distributed
	
	commit cb926e7ea50ad11b8f9e909c05226233bf755030
	Author: Michael Liao <askxuefeng@gmail.com>
	Date:   Mon Aug 19 17:51:55 2013 +0800
	
	    wrote a readme file

**git log** 命令显示从最近到最远的提交日志，我们可以看到3次提交，最近的一次是 **append GPL** ，上一次是 **add distributed**，最早的一次是 **wrote a readme file**。 如果嫌输出信息太多，看得眼花缭乱的，可以试试加上 **--pretty=oneline** 参数：

	$ git log --pretty=oneline
	3628164fb26d48395383f8f31179f24e0882e1e0 append GPL
	ea34578d5496d7dd233c827ed32a8cd576c5ee85 add distributed
	cb926e7ea50ad11b8f9e909c05226233bf755030 wrote a readme file

需要友情提示的是，你看到的一大串类似 **3628164...882e1e0** 的是**commit id**（版本号），和SVN不一样，Git的 **commit id** 不是1，2，3……递增的数字，而是一个SHA1计算出来的一个非常大的数字，用十六进制表示，而且你看到的 **commit id** 和我的肯定不一样，以你自己的为准。为什么 **commit id** 需要用这么一大串数字表示呢？因为Git是分布式的版本控制系统，后面我们还要研究多人在同一个版本库里工作，如果大家都用1，2，3……作为版本号，那肯定就冲突了。

每提交一个新版本，实际上Git就会把它们自动串成一条时间线。如果使用可视化工具查看Git历史，就可以更清楚地看到提交历史的时间线：

好了，现在我们启动时光穿梭机，准备把readme.txt回退到上一个版本，也就是“add distributed”的那个版本，怎么做呢？

首先，Git必须知道当前版本是哪个版本，在Git中，用 **HEAD** 表示当前版本，也就是最新的提交 **3628164...882e1e0** （注意我的提交ID和你的肯定不一样），上一个版本就是 **HEAD^**，上上一个版本就是**HEAD^^**，当然往上100个版本写100个**^**比较容易数不过来，所以写成**HEAD~100**。

现在，我们要把当前版本“append GPL”回退到上一个版本“add distributed”，就可以使用 **git reset** 命令：

	$ git reset --hard HEAD^
	HEAD is now at ea34578 add distributed

**--hard** 参数有啥意义？这个后面再讲，现在你先放心使用。

看看readme.txt的内容是不是版本 **add distributed**：

	$ cat readme.txt
	Git is a distributed version control system.
	Git is free software.

果然。

还可以继续回退到上一个版本 **wrote a readme file**，不过且慢，然我们用 **git log** 再看看现在版本库的状态：

	$ git log
	commit ea34578d5496d7dd233c827ed32a8cd576c5ee85
	Author: Michael Liao <askxuefeng@gmail.com>
	Date:   Tue Aug 20 14:53:12 2013 +0800
	
	    add distributed
	
	commit cb926e7ea50ad11b8f9e909c05226233bf755030
	Author: Michael Liao <askxuefeng@gmail.com>
	Date:   Mon Aug 19 17:51:55 2013 +0800
	
	    wrote a readme file

最新的那个版本 **append GPL** 已经看不到了！好比你从21世纪坐时光穿梭机来到了19世纪，想再回去已经回不去了，肿么办？

办法其实还是有的，只要上面的命令行窗口还没有被关掉，你就可以顺着往上找啊找啊，找到那个 **append GPL** 的 **commit id** 是**3628164...**，于是就可以指定回到未来的某个版本：

	$ git reset --hard 3628164
	HEAD is now at 3628164 append GPL

版本号没必要写全，前几位就可以了，Git会自动去找。当然也不能只写前一两位，因为Git可能会找到多个版本号，就无法确定是哪一个了。

再小心翼翼地看看readme.txt的内容：

	$ cat readme.txt
	Git is a distributed version control system.
	Git is free software distributed under the GPL.

果然，我胡汉三又回来了。

Git的版本回退速度非常快，因为Git在内部有个指向当前版本的 **HEAD** 指针，当你回退版本的时候，Git仅仅是把 **HEAD** 从指向 **append GPL**：

![](http://www.liaoxuefeng.com/files/attachments/001384907584977fc9d4b96c99f4b5f8e448fbd8589d0b2000/0)

改为指向 **add distributed**：

![](http://www.liaoxuefeng.com/files/attachments/001384907594057a873c79f14184b45a1a66b1509f90b7a000/0)

然后顺便把工作区的文件更新了。所以你让 **HEAD** 指向哪个版本号，你就把当前版本定位在哪。

现在，你回退到了某个版本，关掉了电脑，第二天早上就后悔了，想恢复到新版本怎么办？找不到新版本的 **commit id** 怎么办？

在Git中，总是有后悔药可以吃的。当你用 **$ git reset --hard HEAD^** 回退到 **add distributed** 版本时，再想恢复到 **append GPL**，就必须找到 **append GPL** 的 **commit id**。Git提供了一个命令 **git reflog** 用来记录你的每一次命令：

	$ git reflog
	ea34578 HEAD@{0}: reset: moving to HEAD^
	3628164 HEAD@{1}: commit: append GPL
	ea34578 HEAD@{2}: commit: add distributed
	cb926e7 HEAD@{3}: commit (initial): wrote a readme file

终于舒了口气，第二行显示 **append GPL** 的 **commit id** 是3628164，现在，你又可以乘坐时光机回到未来了。

##小结

现在总结一下：

    HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id。

    穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。

    要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。

##工作区和暂存区

Git和其他版本控制系统如SVN的一个不同之处就是有暂存区的概念。

先来看名词解释。

###工作区（Working Directory）

就是你在电脑里能看到的目录，比如我的learngit文件夹就是一个工作区：

![](http://www.liaoxuefeng.com/files/attachments/0013849082162373cc083b22a2049c4a47408722a61a770000/0)

###版本库（Repository）

工作区有一个隐藏目录 **.git** ，这个不算工作区，而是Git的版本库。

Git的版本库里存了很多东西，其中最重要的就是称为 **stage**（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支 **master** ，以及指向 master 的一个指针叫 **HEAD** 。

![](http://www.liaoxuefeng.com/files/attachments/001384907702917346729e9afbf4127b6dfbae9207af016000/0)

分支和 **HEAD** 的概念我们以后再讲。

前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：

第一步是用 **git add** 把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用 **git commit** 提交更改，实际上就是把暂存区的所有内容提交到当前分支。

因为我们创建Git版本库时，Git自动为我们创建了唯一一个 **master** 分支，所以，现在，**git commit** 就是往 **master** 分支上提交更改。

你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

俗话说，实践出真知。现在，我们再练习一遍，先对 **readme.txt** 做个修改，比如加上一行内容：

	Git is a distributed version control system.
	Git is free software distributed under the GPL.
	Git has a mutable index called stage.

然后，在工作区新增一个 **LICENSE** 文本文件（内容随便写）。

先用 **git status** 查看一下状态：

	$ git status
	# On branch master
	# Changes not staged for commit:
	#   (use "git add <file>..." to update what will be committed)
	#   (use "git checkout -- <file>..." to discard changes in working directory)
	#
	#       modified:   readme.txt
	#
	# Untracked files:
	#   (use "git add <file>..." to include in what will be committed)
	#
	#       LICENSE
	no changes added to commit (use "git add" and/or "git commit -a")

Git非常清楚地告诉我们，**readme.txt** 被修改了，而 **LICENSE** 还从来没有被添加过，所以它的状态是 **Untracked**。

现在，使用两次命令 **git add**，把 **readme.txt** 和 **LICENSE** 都添加后，用 **git status** 再查看一下：

	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#       new file:   LICENSE
	#       modified:   readme.txt
	#

Git非常清楚地告诉我们，**readme.txt** 被修改了，而 **LICENSE** 还从来没有被添加过，所以它的状态是 **Untracked**。

现在，使用两次命令 **git add**，把 **readme.txt** 和 **LICENSE** 都添加后，用 **git status** 再查看一下：

	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#       new file:   LICENSE
	#       modified:   readme.txt
	#

现在，暂存区的状态就变成这样了：

![](http://www.liaoxuefeng.com/files/attachments/001384907720458e56751df1c474485b697575073c40ae9000/0)

所以，**git add** 命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行 **git commit** 就可以一次性把暂存区的所有修改提交到分支。

	$ git commit -m "understand how stage works"
	[master 27c9860] understand how stage works
	 2 files changed, 675 insertions(+)
	 create mode 100644 LICENSE


#5 远程仓库

