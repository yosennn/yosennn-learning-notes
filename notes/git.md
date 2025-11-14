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

## 版本回退

### 查看历史提交记录

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

在 git 中，`HEAD` 表示当前版本，`HEAD^` 和 `HEAD^^` 分别代表上一个和上上个版本，往上 100 个版本也可以表示为 `HEAD~100`

使用如下命令回退到上个版本

```git
git reset [--soft | --mixed | --hard] <HEAD> [<file>]
$ git reset --hard HEAD^  # for example
```

其中

- `--soft` 选项，只回退仓库的版本，不影响暂存区和工作目录。一般用于合并多次小提交
- `--mixed` 默认选项，回退仓库版本，清空暂存区，不影响工作目录
- `--hard` 选项，回退仓库版本和工作区目录，清空暂存区。是唯一一个会影响工作目录的选项，较为危险

如果错误回退了版本，可以在 git 窗口未关闭的情况下，找到正确版本的 `commit id`，使用 `git reset <commit id>`，找回版本

## git 文件状态

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



......

## 忽略文件 `.gitignore`

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

在我使用 VPN 时，有 `系统代理` 和 `虚拟网卡` 两个模式。其中单开 `系统代理` 时，浏览器可以正常浏览 github 并下载仓库，但是 git 无法连接 github，而两个个功能都打开时，才能成功连接。

- **系统代理**：是一种应用层的设置，它像一个“告示牌”，告诉电脑上的各个应用程序：“如果你需要访问网络，可以走我这个代理”。但是，是否要看这个“告示牌”并遵守指示，完全取决于应用程序自己。
- **虚拟网卡**：是一种网络层的设置，它直接修改了系统的“交通规则”（路由表），强制将所有的网络流量（或者根据规则设定的流量）都先导向一个虚拟的网络接口，再由VPN软件处理后发出去。这对于应用程序来说是无感知的，也是强制性的。

SSH 协议默认无视操作系统的系统代理设置。SSH 客户端（如 OpenSSH）有自己的代理配置机制，它**不会**自动去读取 `HTTP_PROXY` 或系统代理设置