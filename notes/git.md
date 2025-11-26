# git

> 参考资料
>
> [廖雪峰的 git 教程](https://liaoxuefeng.com/books/git/introduction/index.html)
>
> [阮一峰的网络日志](https://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.htmlAngular)
>
> [git 官网](https://git-scm.com/)
>
> [菜鸟教程：git](https://www.runoob.com/git)
>
> GPT-5 by https://aizex.cn/cw4Y6M
>
> DeepSeek-V3.1 by https://yuanbao.tencent.com/
>
> 豆包 by https://www.doubao.com/chat

git 是当前使用最为广泛的分布式版本控制系统，由 Linus 开发

- 版本控制系统：假设你正在撰写一份演讲稿，在交给领导/老师审核后进行了多次修改。这时领导突然让你使用第一版......或者有多个人一起完成演讲稿（不考虑在线文档），每个人都在原版的基础上增加了属于自己的修改，最后把大家的修改合并在一起就是一个麻烦的事情。而这两个情景，都是版本控制系统解决的问题
- 分布式：分布式代表每个人的电脑上都是一个完整的版本库，可以看到完整的内容。与集中式相对的是分布式，有一个中央服务器存储最新的版本，每次修改都需要从服务器获取最新版本，并把修改情况再推送到服务器。就相当于从图书馆借书（最新版本），然后再还书（推送修改情况）

# git 配置

## 安装

Linux 上，可以直接在命令行输入 `git`。多数 Linux 系统会提醒你没有安装 git 并告诉你如何安装

```shell
$ git
```

对于 Debian 或 Ubuntu Linux，`sudo apt install git` 可以直接完成Git的安装

Windows 上，可以进入 git 官网下载安装程序即可

## 配置

安装好 git 后，需要设置自己的用户信息，包括用户名和 Email

```shell
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

git 也做了防止冒充的措施，不过这里不做展开

注意：`git config` 命令的 `--global` 参数表示，这台机器上所有的 Git 仓库都会使用这个配置。当然也可以对某个仓库指定不同的用户名和Email地址



# 版本库

版本库又常被叫做仓库（Repository），是 git 的核心概念之一

Repository 是项目的档案馆，完整记录这个文件集合的每一次变化历史

## 创建

创建仓库，需要先有一个工作目录。可以通过以下方法创建目录

```sh
$ mkdir learngit  # 创建目录
$ cd learngit
$ pwd
/Users/michael/learngit
```

`pwd` 命令用于显示当前目录，示例中的仓库位于 `/Users/michael/learngit`

下一步，使用 `git init` 把目录变为 git 仓库。当然，你也可以直接在已有目录下创建仓库

```sh
$ git init
Initialized empty Git repository in /Users/michael/learngit/.git/
```

此时，该目录下会多出一个隐藏的 `.git` 目录，git 管理仓库的文件都存放在里面

## 添加文件

首先明确一点——所有的版本控制系统，只适用跟踪文本文件的改动。比如 `.txt` 文件、网页、程序代码等。比如，记录某次改动中，第 5 行增加了单词 `Linux`，第 8 行被删除

而对于图片、视频这些二进制文件，版本控制系统不能显示他们的修改情况

> 千万不要使用Windows自带的**记事本**编辑任何文本文件
>
> 原因是Microsoft开发记事本的团队使用了一个非常弱智的行为来保存 UTF-8 编码的文件——他们自作聪明地在每个文件开头添加了0xefbbbf（十六进制）的字符，你会遇到很多不可思议的问题，比如，网页第一行可能会显示一个“?”，明明正确的程序一编译就报语法错误，等等

进入正题，接着之前的例子，此时的 `git` 仓库刚刚初始化，什么都没有存储。此时目录下新增文件 `README.md` 

### 提交暂存区

第一步，把文件打包进仓库的暂存区

```sh
$ git add README.md
```

没有任何显示说明就成功了，这是来自 Unix 的哲学——“没有消息就是好消息”

`git add` 的常见用法如下

```git
git add <file_1> [<file2> ...]|<dir>  # 添加文件或子目录
git add .  # 添加目录下所有文件
```

### 提交至仓库

第二步，把打包好的文件正式提交到仓库

```sh
$ git commit -m "wrote a readme file"
[master (root-commit) eaadf4e] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
```

`-m` 代表 `message`，后面的字符串代表对本次提交的说明，应包含必要的内容。众多开发者也对该段说明存在一定的规范，可阅读后面提到的 [commit message Angular 规范](#commit message Angular 规范)

`git commit` 的常见用法如下：

```git
git commit [<file1> [<file2>...]] -m "<string>"
# 可以提交暂存区的指定文件到仓库区
```

通过先 `git add` 再 `git commit` 的方式，可以一次性提交多个文件到仓库中，方便管理

## 检查仓库状态

继续上一小章提到的例子，此时已经提交了第一版的 `README.md`。现在，修改 `README.md`

在终端使用 `git status` 可以检测仓库和现有文件的不同

```sh
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

上面的命令输出告诉我们：

- 当前在分支 `master` 上

- `readme.txt` 被修改过；
- 没有已暂存的修改

如果想查看文件的具体修改情况，可以使用 `git diff` 比较文件在暂存区和未进入暂存区，两个版本的不同

```sh
$ git diff readme.txt 
diff --git a/readme.txt b/readme.txt
index 46d49bf..9247db6 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,2 +1,2 @@
-Git is a version control system.
+Git is a distributed version control system.
 Git is free software.

```

在提交修改后，再使用 `git status` 命令，可以看到本次提交内容

```sh
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   readme.txt
```



# 版本控制

git 仓库会保存每一次 `commit`，用户可以随时恢复到以前的版本，就像 RPG 游戏里的存档一样

## 1. 版本回退

###  查看历史提交记录

 `git log` 可以查看历史提交记录。示例：

```sh
$ git log
commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:06:15 2018 +0800

    append GPL

commit e475afc93c209a690c39c13a46716e8fa000c366
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:03:36 2018 +0800

    add distributed

commit eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 20:59:18 2018 +0800

    wrote a readme file
```

示例中，类似 `eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0` 的一大串字符是每次提交的 `commit id`

[菜鸟教程](https://www.runoob.com/git/git-commit-history.html#git-log) 里有更多 `git log` 的可选参数

### 版本回退

想回复到之前的版本，需要知道如何表示目标版本

在 git 中，先后提交的不同版本在逻辑上组成一个链表。`HEAD` 可以理解为指向当前版本的指针，`HEAD^` 和 `HEAD^^` 分别代表上一个和上上个版本，往上 100 个版本也可以表示为 `HEAD~100`

使用如下命令回退到上个版本

```git
git reset [--soft | --mixed | --hard] <HEAD> [<file>]
$ git reset --hard HEAD^  # for example
```

其中

- `--soft` 选项，只回退仓库的版本，不影响暂存区和工作目录。一般用于合并多次小提交
- `--mixed` 默认选项，回退仓库版本，清空暂存区，不影响工作目录
- `--hard` 选项，回退仓库版本和工作区目录，清空暂存区。是唯一一个会影响工作目录的选项，较为危险
- `<HEAD>` 用于指明要回退到哪个版本
- `<file>` 可以指定只回退某个具体的文件

如果错误回退了版本，可以在 git 窗口未关闭的情况下，找到正确版本的 `commit id`，使用 `git reset <commit id>`，找回版本

## 2. git 文件状态

git 的文件状态分为三种：工作目录（Working Directory）、暂存区（Staging Area）、本地仓库（Local Repository）

### 工作目录

工作目录是你在本地计算机上能直接查看和编辑的目录。所有对文件的更改首先发生在工作目录中

在工作目录中的文件可能有以下几种状态：

- **未跟踪（Untracked）**：新创建的文件，未被 Git 记录。
- **已修改（Modified）**：已被 Git 跟踪的文件发生了更改，但这些更改还没有被提交到 Git 记录中。

### 暂存区

暂存区，也称为索引（Index），是一个临时存储区域，用于保存即将提交到本地仓库的更改

### 本地仓库

本地仓库是一个隐藏在 `.git` 目录中的数据库，用于存储项目的所有提交历史记录。每次提交更改时，git 会将暂存区中的内容保存到本地仓库中

### 文件状态的转换

**未跟踪（Untracked）**： 新创建的文件最初是未跟踪的。它们存在于工作目录中，但没有被 Git 跟踪。

**已跟踪（Tracked）**： 通过 `git add` 命令将未跟踪的文件添加到暂存区后，文件变为已跟踪状态。

**已修改（Modified）**： 对已跟踪的文件进行更改后，这些更改会显示为已修改状态，但这些更改还未添加到暂存区。

**已暂存（Staged）**： 使用 `git add` 命令将修改过的文件添加到暂存区后，文件进入已暂存状态，等待提交。

**已提交（Committed）**： 使用 `git commit` 命令将暂存区的更改提交到本地仓库后，这些更改被记录下来，文件状态返回为已跟踪状态。

## 3. 撤销更改

假设在某次 `commit` 过后，你修改了 `file_1` 文件的内容。后面需要撤销该文件的修改，回到 `commit` 的版本

按照 `file_1`  的提交情况，可以分为以下几类

### 工作区的修改

如果文件还没暂存，可以使用如下命令**丢弃工作区的修改**

```bash
git checkout -- <file_name>
$ git checkout -- file_1
```

意思就是，把文件在工作区的修改全部撤销，恢复到和仓库一模一样的状态

> 实际上 `git checkout` 的语义是切换到另一个分支

如果文件在暂存后，又被修改了内容。即仓库里是 `v1`、暂存区是 `v2`、工作区是 `v3`，则使用 `git checkout -- <file_name>` 会让工作区的文件从 `v3` 恢复到 `v2`

### 暂存区的修改

如果修改后的文件已经暂存，但还未提交，则可以使用 `git reset` 

```bash
$ git reset HEAD file_1
```

使用后，暂存区的修改被清除

`git reset` 的用法在之前的章节[版本回退](###版本回退)中介绍过

## 4. 从仓库删除文件

假设你要删除某个已被提交到仓库的文件 `file_1`。虽然从工作目录删除了，但是仓库中仍然保存着这个文件，`git status` 仍然可以看到这个文件

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	deleted:    file_1

no changes added to commit (use "git add" and/or "git commit -a")

```

最简单的方法是使用 `git add` 把更改暂存然后再提交

这里介绍 `git rm <file_name>`，它会将文件从暂存区和工作区中删除，省略了手工删除的步骤

```
git rm [-f|--cached] <file_name>
```



另一种情况是你把某个提交过的文件误删了，这时 `git checkout -- <file_name>` 可以把文件恢复到仓库中的版本



# 远程仓库

仓库除了存放在本地，还可以托管在服务器上。在团队协作时，这样方便统一管理

有一些知名的网站提供了代码托管服务，例如 [GitHub](https://github.com)、GitLab、[Gitee](https://gitee.com/)。这里的介绍以 GitHub 为例

## 1. SSH 设置

本地 git 仓库和 GitHub 仓库之间的传输是通过 SSH 加密的，所以，需要设置 SSH

### 1.1. 创建 SSH Key

在用户目录下的 `~` 的 `.ssh` 目录下，如果没有 `id_rsa` 和 `id_rsa.pub`，则需要打开 `Shell`（windows 下的 `git bash`）输入以下命令创建 SSH Key

```sh
$ ssh-keygen -t rsa -C "<email>"
```

`<email>` 自行替换为自己的电子邮箱。接下来一路回车即可

### 1.2. 设置 github

上一步完成后，你已经在 `~/.ssh` 下有了 `id_rsa` 和 `id_rsa.pub` 两个文件

登入 github 的账号，打开 `Account settings` 下的 `SSH Keys` 选项，把 `id_rsa.pub` 里的 key 添加进去

> `id_rsa` 为私钥，把数据加密为密文。`id_rsa.pub` 为该私钥配对的唯一公钥，可以把密文解密
>
> 这样，github 就能确定是来自你的提交

为了验证是否成功，可以输入以下命令

```sh
$ ssh -T git@github.com
```



## 2. 添加远程库

如果你想通过 git 分享你的代码或者与其他开发人员合作，最好将数据放到一台其他开发人员能够连接的服务器上。这就涉及 git 中的远程仓库

在 github 中新建一个空仓库，假设仓库名称为 `learngit`。可以选择推送本地仓库，也可以选择从 github 上克隆到本地

在本地通过如下命令添加该远程库

```sh
$ git remote add origin git@github.com:<github_name>/learngit.git
```

（`<github_name>` 为你自己的 git 用户名）

添加后，远程库的名字就是 `origin`。这是默认叫法，不推荐随意更改

下一步，就可以把本地库的所有内容推送到远程库上：

```sh
git push <远程仓库名> <本地分支名>:<远程分支名>
$ git push -u origin master
```

其中，

- 由于 `<本地分支名>:<远程分支名>` 是相同的，所以只保留一个

- `-u` 代表 `--set-upstraem`，可以建立本地分支与远程分支的追踪关系。以后在这个分支上就可以直接使用 `git push` 推送和 `git pull` 拉取，简化操作，而无需再指定远程仓库和分支名称

添加后，也可以在本地查看已有的远程库：

```sh
git remote [-v]
$ git remote
origin
$ git remote -v
origin    git@github.com:tianqixin/runoob-git-test.git (fetch)
origin    git@github.com:tianqixin/runoob-git-test.git (push)
```

执行时加上 `v` 参数，还可以看到每个别名的实际链接地址

## 3. SSH 警告

当你第一次使用Git的`clone`或者`push`命令连接GitHub时，会得到一个警告

```sh
The authenticity of host 'github.com (xx.xx.xx.xx)' can't be established.
RSA key fingerprint is xx.xx.xx.xx.xx.
Are you sure you want to continue connecting (yes/no)?
```

这是因为 Git 使用 SSH 连接，而 SSH 连接在第一次验证 GitHub 服务器的 Key 时，需要你确认 GitHub 的 Key 的指纹信息是否真的来自 GitHub 的服务器

输入 `yes` 回车即可。Git 会再输出一个警告，告诉你已经把GitHub的 Key 添加到本机的一个信任列表里了

## 4. 删除远程库

如果想删除远程库，可以用 `git remote rm <name>` 命令。使用前，建议先用 `git remote -v` 查看远程库的名字

然后，根据名字删除。比如删除`origin`：

```sh
$ git remote rm origin
```

此处的“删除”其实是解除了本地和远程的绑定关系，远程库本身并没有任何改动

## 5. 从远程库克隆

`git clone` 可以将远程仓库的所有分支和历史记录复制到本地，在当前目录创建一个与远程仓库同名的文件夹

```sh
git clone <url>
```

其中，`<url>` 支持 `ssh` 和 `https` 协议的传输，例如

- `git@github.com:michaelliao/gitskills.git`
- `https://github.com/michaelliao/gitskills.git`

`https` 传输较慢，推荐使用 `ssh` 协议的链接



# 分支管理

分支（branch）是 git 强大功能之一，能够让多个开发人员并行编程而不影响主代码库

使用分支可以理解为创建了一个代码的不同时间线，从开发主线上分离开来。后续以一定的规则合并到主线

## 指针概念

git 的分支实际上是指向更新快照的指针

> 数据存储领域，快照指对特定数据的一份拷贝，记录数据再某个时间点的状态

这里介绍几个重要的指针概念

在不涉及多分支的情况下，git 的不同版本均被串成一条时间线，不同的提交按照先后顺序在时间线上排列。而这里的时间线就是一个分支，且是一个特殊的分支——主分支

主分支通常由 `master` 指针（也可能是 `main` 指针）指定，随着新的提交而自动向前移动

`HEAD` 则指向你当前的工作位置，通常指向某个分支指针。`git status` 就是以 `HEAD` 指向的版本为基准，来检查文件状态的

## 实例分析

一开始的时候，`master` 分支是一条线。`HEAD` 指向 `master`，就能确定当前分支，以及当前分支的提交点

```
                  HEAD
                    │
                    ▼
                 master
                    │
                    ▼
┌───┐    ┌───┐    ┌───┐
│   │───▶│   │───▶│   │
└───┘    └───┘    └───┘
```

当我们创建新分支 `dev` 时，Git新建了一个叫 `dev` 的指针，指向 `master` 相同的提交。再把 `HEAD` 指向 `dev`，就表示当前分支在 `dev` 上

```
            master
               │
               ▼
    ┌───┐    ┌───┐
───▶│   │───▶│   │
    └───┘    └───┘
               ▲
               │
              dev
               ▲
               │
             HEAD
```

由于 `HEAD` 指向 `dev`，后面对工作区的修改和提交就是针对`dev`分支的。比如提交了两个版本后：

```
   master
      │
      ▼
    ┌───┐    ┌───┐    ┌───┐
───▶│ x │───▶│x+1│───▶│x+2│
    └───┘    └───┘    └───┘
                        ▲
                        │
                       dev
                        ▲
                        │
                      HEAD
```

假如我们在 `dev` 上的工作完成了，就可以把 `dev` 合并到 `master` 上。最简单的合并方法就是直接把 `master` 指向 `dev` 的当前提交

合并完分支后，甚至可以删除 `dev` 分支。删掉后，就只剩下了一条 `master` 分支。`dev`分支上的提交历史都可以在 `master` 中看到

> 如果使用了特定的参数，可以让 `master` 不记录另一条分支的提交历史。从逻辑上看，`dev` 分支的多次提交在 `master` 中只剩下了一条：

```
             HEAD
               │
               ▼
            master
               │
               ▼
    ┌───┐    ┌───┐
───▶│x+1│───▶│x+2│
    └───┘    └───┘
```

## 创建与切换分支

创建并切换到 `dev` 分支

```sh
git checkout -b dev
```

使用 `-b` 参数相当于如下两条命令

```sh
git branch <branch_name>
git checkout <branch_name>
```

当你切换分支的时候，git 会用该分支的最后提交的快照替换你的工作目录的内容

## 查看分支

查看所有分支，在当前分支前会标一个 `*` 号

```sh
git branch [-r|-a]
```

参数 `-r` 查看远程分支

参数 `-a` 查看所有远程和本地分支

## 分支合并与删除

将其他分支合并至当前分支：

```sh
git merge <branch_name>
```

即把当前分支的指针指向 `<branch_name>` 所指的分支。工作区的内容也会同时修改

合并后，删除指定的分支：

```sh
git branch -d <branch_name>
```

## git switch

注意到，切换分支使用 `git checkout <branch>`，而前面讲过的撤销修改则是 `git checkout -- <file>`，两者在语义上有冲突

为了更清晰的切换分支，`git 2.23` 提供了 `git switch` 命令，专门用于切换分支，还提供了更好的错误检查

```sh
git switch [-c|-f|-d] <branch_name>|<commit_hash>
```

`-c` 参数即 `--create`，表示创建并切换

## 冲突解决

假设开发者创建了分支 `feature`，在 `master` 上的 `x` 文件做了修改 `i`，在 `feature` 上的 `x` 文件做了修改 `j`

这时，git 的分支不再是一条线，而是出现了分叉：

```
             master
                │
                ▼
              ┌───┐
           ┌─▶│x+i│
    ┌───┐  │  └───┘
───▶│ x │──┤
    └───┘  │  ┌───┐
           └─▶│x+j│
              └───┘
                ▲
                │
             feature
```

这种情况下，git 无法执行“快速合并”，只能尝试把各自的修改合并起来。但这种合并就可能会有冲突，比如两个分支修改了同一行代码

```sh
$ git merge feature1
Auto-merging readme.md
CONFLICT (content): Merge conflict in readme.md
Automatic merge failed; fix conflicts and then commit the result.
```

根据报错 `CONFLICT`，git 在合并 `readme.md` 文件时存在冲突。git 会直接修改文件，打开文件就可以看到冲突信息，例如：

```sh
$ cat readme.md
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
<<<<<<< HEAD
Creating a new branch is quick & simple.
=======
Creating a new branch is quick AND simple.
>>>>>>> feature

```

`<<<<<<<`，`=======`，`>>>>>>>` 分别标记出不同分支的内容。`=` 似乎表示占位，而 `<` 表示从下一行到 `=` 之间的内容属于 `HEAD`，`>` 表示从 `=` 之间到 `>` 上一行的内容属于 `feature`

这时需要人工解决，可以自动把另一分支的文件变动合并至当前分支并提交，这样两个分支就兼容了

解决后，使用 `git add` 告诉 git，文件冲突已经解决

```sh
$ git add readme.txt 
$ git commit -m "conflict fixed"
[master cf810e4] conflict fixed
```

分支变为

```
                       master
                          │
                          ▼
              ┌───┐     ┌───┐
           ┌─▶│x+i│───▶│x+i+j│
    ┌───┐  │  └───┘     └───┘
───▶│ x │──┤              ▲
    └───┘  │  ┌───┐       │
           └─▶│x+j│───────┘
              └───┘
                ▲
                │
             feature
```







...

## 忽略指定文件

`.gitignore` 文件是一个存放在项目根目录下的纯文本文件，指定 git 应忽略哪些文件或文件夹







# commit message Angular 规范

[阮一峰的网络日志](https://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.htmlAngular) 介绍了 Angular 规范

该规范认为，每次提交的 commit message 都包含三部分：`type, Body, Footer`

```
<type>(<scope>): <subject>
// 空一行
<body>
// 空一行
<footer>
```

其中，`Header` 是必须的，剩余两个都可以省略

### Header

Header 部分只有一行，包括三个字段：`type`（必需）、`scope`（可选）和`subject`（必需）

```
<type>[(<scope>)]: <subject>
```



#### type

`type` 字段用于说明 commit 的类别，包括 7 种：

- feat：新功能（feature）
- fix：修补 bug
- docs：文档（documentation），不涉及代码逻辑，针对 “文字说明”（文档、注释）
- style： 格式（不影响代码运行的变动），代码风格的更改，不影响程序逻辑。针对 “代码格式”，例如函数命名、换行
- refactor：重构（即不是新增功能，也不是修改bug的代码变动），优化代码结构、逻辑或命名，提高可读性、复用性，但不改变功能
- perf：性能优化
- test：新增、修改或修复测试代码
- chore：对构建、配置、脚本或依赖的调整，不影响代码逻辑和功能
- build：构建系统或外部依赖变化

如果`type`为`feat`和`fix`，则该 commit 将肯定出现在 Change log 之中

（当一次提交包含了多种类型的更改（例如同时增加了新功能和补充了单元测试），核心的处理原则是：根据提交对项目产生的实际影响，选择一个最主要、最核心的变更类型作为本次提交的 `type`，然后在提交信息的正文（Body）中详细描述所有具体的更改内容）

#### scope

`scope`用于说明 commit 影响的范围，比如数据层、控制层、视图层等等，视项目不同而不同

#### subject

`subject`是 commit 目的的简短描述，不超过50个字符

### body

Body 部分是对本次 commit 的详细描述，可以分成多行。下面是一个范例。

 ```bash
 More detailed explanatory text, if necessary.  Wrap it to 
 about 72 characters or so. 
 
 Further paragraphs come after blank lines.
 
 - Bullet points are okay, too
 - Use a hanging indent
 ```

有两个注意点。

（1）使用第一人称现在时，比如使用 `change` 而不是`changed`或`changes`

（2）应该说明代码变动的动机，以及与以前行为的对比。

### Footer

Footer 部分只用于两种情况：不兼容变动和关闭 Issue

- 如果当前代码与上一个版本不兼容，则 Footer 部分以`BREAKING CHANGE`开头，后面是对变动的描述、以及变动理由和迁移方法。

```bash
BREAKING CHANGE: isolate scope bindings definition has changed.

    To migrate the code follow the example below:

    Before:

    scope: {
      myAttr: 'attribute',
    }

    After:

    scope: {
      myAttr: '@',
    }

    The removed `inject` wasn't generaly useful for directives so there should be no code using it.
```

- **关闭 Issue**

  如果当前 commit 针对某个issue，那么可以在 Footer 部分关闭这个 issue 。

 ```bash
 Closes #234, #123
 ```

### Revert

还有一种特殊情况，如果当前 commit 用于撤销以前的 commit，则必须以`revert:`开头，后面跟着被撤销 Commit 的 Header。

 ```bash
 revert: feat(pencil): add 'graphiteWidth' option
 
 This reverts commit 667ecc1654a317a13331b17617d973392f4
 ```

Body部分的格式是固定的，必须写成 `This reverts commit <hash>.`，其中的 `hash` 是被撤销 commit 的 SHA 标识符。



# 遇到的问题

## github 贡献者显示问题

github 上显示的提交者并非自己的账号

- 描述：拉取 github 仓库后再本地修改，使用 vscode 推送远程仓库后，github 端无法识别贡献者
- 解决：询问 GPT-5 得知解决方法。修改本地 git user.name，github 增加本地 git 仓库的 email，检查 `git remote -v` 后删除多余的仓库名

## git 连接问题

在我使用 VPN 时，有 `系统代理` 和 `虚拟网卡` 两个模式。其中单开 `系统代理` 时，浏览器可以正常浏览 github 并下载仓库，但是 git 无法连接 github，而两个个功能都打开时，才能成功连接

- **系统代理**：是一种应用层的设置，它像一个“告示牌”，告诉电脑上的各个应用程序：“如果你需要访问网络，可以走我这个代理”。但是，是否要看这个“告示牌”并遵守指示，完全取决于应用程序自己。
- **虚拟网卡**：是一种网络层的设置，它直接修改了系统的“交通规则”（路由表），强制将所有的网络流量（或者根据规则设定的流量）都先导向一个虚拟的网络接口，再由VPN软件处理后发出去。这对于应用程序来说是无感知的，也是强制性的。

SSH 协议默认无视操作系统的系统代理设置。SSH 客户端（如 OpenSSH）有自己的代理配置机制，它**不会**自动去读取 `HTTP_PROXY` 或系统代理设置