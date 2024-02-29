## Git

### 版本控制

版本控制系统（VCS）：一种记录一个或若干文件内容变化，以便查阅特定版本修订情况的系统

优势：
- 回溯文件的历史状态和历史版本
- 比较变化
- 修改 -> 恢复

演变：
1. 本地（文件记录）
2. 集中化（集中管理、协同修改更新）
3. 分布式（服务器和客户端扮演相同角色）

主流分布式版本控制工具：Git
主流集中式版本控制系统：SVN

Git 和 SVN 的区别：
+ Git 是集中式版本控制系统，SVN 是分布式版本控制系统
+ SVN 的版本库放在中央服务器上，Git 没有中央服务器
+ SVN 需要联网，Git 不需要

### 初步

#### 出现原因

Linux 内核开源的维护工作花费在了繁琐的文档上，2002，项目组启用了一个专有的分布式版本控制系统来管理和维护代码（BitKeeper，开源社区和某商业公司合作的）。2005年合作结束后（被迫），Linux开源社区自己开发的分布式版本控制系统（即现在的Git）

#### 特点

1. 快照而非差异
大部分其他的版本控制系统都是以文件变更列表的方式存储信息（存储信息的视角是：一组文件和每个文件基于时间累计的差异）
Git是以文件快照的方式存储信息（存储信息的视角：一组不同版本的文件快照通过版本链形成的快照流）
2. 本地执行
绝大多数操作只需要访问本地的文件和资源，在没有网络链接的情况下也可以完成大部分的工作（版本对比、版本查看、本地提交等等）
3. 保证完整性
所有数据在存储前都计算了校验和，通过校验和来引用（文件目录的更新都是可知的，通过SHA-1散列出40个十六进制字符组成的字符串，是基于文件和目录结构计算出来的）
4. 只添加数据
任何的操作都基本都会进行记录（即添加，目的是为了查看历史记录和撤销）
5. 三种状态
已提交：数据已经安全的保存在了数据库
已修改：内容修改，但未提交到数据库
已暂存：对已修改的文件的当前版本做了标记，使之包含在提交中

#### 安装

官网：https://git-scm.com/

下载网址：https://git-scm.com/downloads

#### 配置

命令：`git config`

配置分三个优先级目录，级别之间进行覆盖
1. `/etc/gitconfig`  通过携带参数`--system`来配置用户和仓库的通用配置
2. `~/.gitconfig` 或  `~/.config/git/config` 只针对当前用户，通过参数 `--global` 来生效
3. 当前仓库的`.git/config`文件下的配置，只针对当前仓库，通过 `--local` 来生效
例如，配置用户信息和编辑器：
```
git config --global user.name "alateer"
git config --blobal user.email xx@gmail.com
git config --global core.editor="D:\code\vscode\bin\code"
```

命令：` git config --list --show-origin`

显示配置信息及所在文件, `--show-origin` 会显示所有的三个级别

#### 获取帮助

命令：
`git help <verb>`

`git <verb> --help`


### 基础

#### 获取Git仓库

1. 将尚未进行版本控制的本地目录转换为 Git 仓库
命令： `git init`

2. 从其他服务器 克隆 一个已存在的 Git 仓库
命令： `git clone <url> <local-response-name>`


#### 记录更新

查看当前仓库的文件状态：`git status`

跟踪/暂存文件命令：`git add <file | *> -s`

仓库的文件状态变化周期

- Untracked(未跟踪)： 

意味着 Git 在之前的快照（提交）中没有这些文件，Git不会自动纳入跟踪范围

- Unmodified(未修改) | Modified(已修改)

当被追踪的文件被修改以后，文件的状态会成为Unmodified，通过暂存命令让修改的文件成为暂存状态

- Stated(已暂存)

通过`git add`后，文件会被放置在暂存区，暂存的当前所有修改的快照（暂存之后可以继续修改，但修改并不在暂存的版本里面）。每次的提交（commit）都是记录暂存区的结果


查看未暂存文件改动的详细信息：`git diff`
后跟`-- staged` 命令查看已赞存和提交的内容的改动区别

提交更新：`git commit`
直接commit会打开默认编辑器，保存后即可完成提交
后跟`-m`参数，直接书写提交信息
后跟`-a`参数，跳过使用暂存区

移除文件：`git rm`
移动文件：`git mv`

#### 忽略文件

通过记录 `.gitignore` 文件来记录不进行跟踪的文件列表
使用标准的 glob 模式来匹配文件
一个仓库根目录下只需要有一个 `.gitignore` 文件，它递归地应用到整个仓库中


#### 查看提交历史

查看提交历史：`git log`
- 无参数情况下，按时间顺序返回最近的所有提交
- 参数`-p` or `--patch`，会显示每次提交所引入的差异
- 参数 `-2` 限制显示条数
- 










### Git基本理论结构（核心）

三个本地区域

+ 工作目录 checktout
+ 暂存区 add reset
+ 仓库区 commit pull

一个远程区域

+ 远程仓库区 push 

### Git项目搭建

两种方式

+ 本地仓库搭建

```bash
git init
```

+ 克隆远程仓库

```bash
git clone [url]
```

### Git文件操作

+ 状态查看

```bash
git status
```

+ 将所有文件添加到暂存区

```bash
git add .
```

+ 将所有文件从暂存区添加到本地仓库区

```bash
git commit -m "..."
```

+ 将所有文件从本地仓库区推到远程仓库区

```bash
git add .
```

### 忽略文件

在主目录下建立".gitignore"文件，在该文件中添加一些规则，可以将某些文件忽略不纳入版本控制中

例如：

```
### IntelliJ IDEA ###
.idea
*.iws
*.iml
*.ipr

### NetBeans ###
/nbproject/private/
/build/
/nbbuild/
/dist/
/nbdist/
/.nb-gradle/
```

## old-git-note

### 创建版本库
1. 在一个适当的地方创建一个空目录（命令包括：mkdir、cd、pwd）
2. 通过`git init`命令把这个目录变成Git可以管理的仓库（目录中包含一个隐藏文件.git)
3. 把想要添加的文件添加到git仓库中，分为两步
   1）使用`git add 文件`命令告诉git把文件添加到仓库中；
   2)使用命令`git commit -m "描述提交文件的说明"`告诉git，把文件提交到仓库

### 仓库状态与修改查看
```bash
$ git status
```
该命令用于让我们时刻掌握仓库目前的状态，是否有文件被修改或被提交
```bash
$ git diff 含有修改内容的文件
```
查看difference，即修改的内容

### 版本查看及版本回退
```bash
$ git log
```
显示最近到最远的提交日志（即提交过的不同版本）
如果日志数量太多，可以加上`--pretty=oneline`参数
```bash
$ git reset --hard HEAD^
```
在git中，用HEAD表示当前版本，每个版本都有一个对应的commit id（一大长串的16进制数）
上一个版本就是HEAD^，上上一个是HEAD^^，大一些的往上版本可以写成HEAD~xxx
git reset命令作用是回退版本
```bash 
$ git reflog
```
记录每一次的命令
可用于找到恢复版本的commit id，以便于查找
注：`cat filename`用于查看文本文件内容

### 工作区和暂存区
+ 工作区（working directory)：就是电脑中能看到的目录，比如一个learngit文件夹就是一个工作区
+ 版本库（repository)：在工作区中含有隐藏文件.git，这就不是工作区，是git的版本库
+ 暂存区（stage or index)：存在于git版本库中最重要的东西
+ 另git还为版本库中创建了第一个分支master，以及指向master的一个指针HEAD
+ 命令git add是把文件从工作区添加到暂存区
+ 命令git commit提交更改，实际就是把暂存区的所有内容提交到当前分支（可见分支可建多个）、
+ 需要提交的文件修改通通放到暂存区，然后一次性提交暂存区的所有修改

### 修改的管理与撤销
可以通过使用`git diff HEAD -- filename`命令查看工作区和版本库在最新版中的区别
```bash
$ git checkout -- file
```
该命令用于当你修改了工作区的某个文件内容后，想丢弃这个修改时
```bash
$ git reset HEAD <file>
```
该命令用于当你的修改已经add到了暂存区，把文件重新退回到工作区，再使用checkout命令放弃修改

### 删除文件
在没有将文件添加到版本库之前（add and commit），一般的文件删除使用`rm filename`即可，该文件就会被完全删除
```bash
$ git rm filename
```
该命令用于删除已经提交到版本库的文件（执行该命令后，再commit一下）
若文件提交到版本库后，删除掉了工作区的文件，则使用恢复命令checkout即可

 ### 远程仓库
 远程仓库一般使用github来进行远程同步，可以将本地仓库的更改更新到远程的guthub仓库，也可以克隆最新的仓库更新到本地
 ```bash
$ git remote add origin git@server-name:path/repo-name.git
$ git push -u origin master
$ git push origin master
 ```
第一个命令用于关联一个远程库，origin是一个默认远程库名
第二个命令用于第一次推送master分支的所有内容
第三个命令只需在想更新2推送最新修改时使用
```bash
$ git clone git@server-name:path/repo-name.git
```
该命令用于克隆一个仓库
git支持多种协议，包括https，但通过ssh支持的原生git协议速度更快

### 创建与合并分支
+ 分支创建（两种方法）
```bash
$ git checkout -b dev

$ git branch dev
$ git checkout dev
```
-b参数表示创建并切换

+ 查看当前分支命令
```bash
$ git branch
```
*号标出的分支即为当前分支

+ 将子分支的工作合并到master分支上
```bash
$ git merge dev
```
前提是当前分支是master，要合并的是dev

+ 删除分支
```bash
$ git branch -d dev
```

### 分支管理
+ `git branch -v`命令可以查看每一个分支的最后一次提交
+ 检测那些分支是否提交到当前分支上
```bash
$ git branch --merged
$ git branch --no-merged
```
第一个命令查看那些分支已经合并到当前分支（即可以删除）
第二个命令查看哪些分支还未合并到当前分支（删除会有提示）

##