# Git-2022.7.21

是一种分布式版本控制工具，支持开发人员在不同的机器，不同的地方对同一仓库的代码进行合作开发。共分为工作区workspace、暂存区index、资源库repository和远端remote

## 文件状态

- Untracked：未跟踪状态。此文件在文件夹中，但并没有加入到git库，不参与版本控制。通过git add 状态变为Staged。
- Staged：暂存状态。执行git commit则将修改同步到库中, 这时库中的文件和本地文件又变为一致，文件为Unmodify状态。执行git reset HEAD filename取消暂存，文件状态为Modified。

- Unmodify: 文件已经入库。未修改，即版本库中的文件快照内容与文件夹中完全一致。这种类型的文件有两种去处，如果它被修改，而变为Modified。如果使用git rm命令移出版本库，则成为Untracked文件。
- Modified：文件已修改，仅仅是修改，并没有进行其他的操作。这个文件也有两个去处，通过git add可进入暂存staged状态，使用git checkout 则丢弃修改。返回到unmodify状态, 这个git checkout即从库中取出文件, 覆盖当前修改。

## Git 基本操作order

### git init

```shell
git init  # 使用当前目录作为 Git 仓库，我们只需使它初始化。
git init newrepo  # 以newrepo为文件名在当前目录下创建文件夹并初始化Git仓库
```

```shell
# 如果当前目录下有几个文件想要纳入版本控制，需要先用 git add 命令告诉 Git 开始对这些文件进行跟踪，然后提交：
$ git add *.c
$ git add README
$ git commit -m '初始化项目版本'
```

### git clone

```shell
git clone <repo>  # 从现有 Git 仓库中拷贝项目
git clone <repo> <directory>  # 克隆到指定的目录

# 自己定义要新建的项目目录名称，可以在上面的命令末尾指定新的名字
$ git clone git://github.com/schacon/grit.git mygrit # 从git网址中clone项目到本地并修改名为mygrit
```

### git config

```shell
git config --list  # 显示当前的 git 配置信息
```

<img src="https://raw.githubusercontent.com/Soooooophia/Sooophia_Picgo/master/image-20220721092859978.png" alt="image-20220721092859978 " style="zoom:50%;" />

```shell
git config -e    # 针对当前仓库，编辑 git 配置文件
git config -e --global   # 针对系统上所有仓库，编辑 git 配置文件

# 设置提交代码时的用户信息：
$ git config --global user.name "runoob"
$ git config --global user.email test@runoob.com
```

### git add

```shell
git add [file1] [file2] ...  # 添加一个或多个文件到暂存区
git add [dir]  # 添加指定目录到暂存区，包括子目录
git add .  # 添加当前目录下的所有文件到暂存区
```

<img src="https://raw.githubusercontent.com/Soooooophia/Sooophia_Picgo/master/image-20220721094048754.png" alt="image-20220721094048754 " style="zoom:50%;" />

~~~markdown
1. git status -s查询当前项目的状态
2. 文件状态说明
		?? 文件未被追踪
		A 文件被追踪，已在暂存区
		AM 状态的意思是这个文件在我们将它添加到缓存之后又有改动。
~~~

### git status

~~~ shell 
git status # 用于查看在你上次提交之后是否有对文件进行再次修改
git status -s # 使用 -s 参数来获得简短的输出结果
~~~

<img src="https://raw.githubusercontent.com/Soooooophia/Sooophia_Picgo/master/image-20220721095306251.png" alt="image-20220721095306251 " style="zoom:50%;" />

### git diff

```shell
git diff [file]  # 显示暂存区和工作区的差异
git diff --cached [file] | git diff --staged [file] # 显示暂存区和上一次提交(commit)的差异
git diff HEAD  # 查看已缓存的与未缓存的所有改动
git diff --stat  # 以摘要形式显示改动
git diff [first-branch]...[second-branch]  # 显示两次提交之间的差异
```

<img src="https://raw.githubusercontent.com/Soooooophia/Sooophia_Picgo/master/image-20220721101228921.png" alt="image-20220721101228921 " style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/Soooooophia/Sooophia_Picgo/master/image-20220721102025270.png" alt="image-20220721102025270 " style="zoom:50%;" />

如果修改文件中的一个字符，使用diff命令的显示结果为

<img src="https://raw.githubusercontent.com/Soooooophia/Sooophia_Picgo/master/image-20220722090819910.png" alt="image-20220722090819910" style="zoom:50%;" />

### git commit

```shell
git commit -m [message]  # 将暂存区内容添加到本地仓库中
git commit [file1] [file2] ... -m [message] # 提交暂存区的指定文件到仓库区
git commit -am [message] # -a 参数设置修改文件后不需要执行 git add 命令，直接提交

# 例如
$ git commit -am '修改 hello.php 文件'
[master 71ee2cb] 修改 hello.php 文件
 1 file changed, 1 insertion(+)
```

~~~shell
git commit -amend  # 修改最近一次提交的message；也可以在不提交新的commit-id的情况下实现追加提交-->见情景使用实例
~~~

### git reset

```shell
git reset [--soft | --mixed | --hard] [HEAD]  

# --mixed 为默认，可以不用带该参数，用于重置暂存区的文件与上一次的提交(commit)保持一致，工作区文件内容保持不变。
git reset  [HEAD] 
$ git reset HEAD^            # 回退所有内容到上一个版本  
$ git reset HEAD^ hello.php  # 回退 hello.php 文件的版本到上一个版本  
$ git  reset  052e           # 回退到指定版本

# --soft 参数用于回退到某个版本
git reset --soft HEAD
$ git reset --soft HEAD~3   # 回退上上上一个版本 

# --hard 参数撤销工作区中所有未提交的修改内容，将暂存区与工作区都回到上一次版本，并删除之前的所有信息提交
git reset --hard HEAD
$ git reset --hard HEAD~3  # 回退上上上一个版本  
$ git reset -hard bae128  # 回退到某个版本回退点之前的所有信息。 
$ git reset --hard origin/master    # 将本地的状态回退到和远程的一样 
```

关于HEAD

~~~markdown
1. 用箭头标识
  HEAD 表示当前版本
  HEAD^ 上一个版本
  HEAD^^ 上上一个版本
  HEAD^^^ 上上上一个版本
  以此类推...
2. 可以使用{～数字}表示
  HEAD~0 表示当前版本
  HEAD~1 上一个版本
  HEAD^2 上上一个版本
  HEAD^3 上上上一个版本
  以此类推...
~~~

<img src="https://raw.githubusercontent.com/Soooooophia/Sooophia_Picgo/master/image-20220721110933925.png" alt="image-20220721110933925" style="zoom:50%;" />

~~~markdown
在已经提交本地仓库的hello.php和README文件进行修改后，两个文件的状态会变成“尚未暂存以备提交的变更“（红色的M），当使用git add命令将两个文件提交缓存后，状态变为”要提交的变更“（绿色的M），reset命令就是将变为绿色的M打回到红色的M状态的过程；而此时下一步若执行commit命令则只会提交README文件的变更
~~~

### git rm

```shell
git rm <file>  # 将文件从暂存区和工作区中删除
git rm -f <file>  # 强行从暂存区和工作区中删除修改后的 runoob.txt文件 :如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 -f.
git rm --cached <file>  # 如果想把文件从暂存区域移除，但仍然希望保留在当前工作目录中，换句话说，仅是从跟踪清单中删除，使用 --cached 选项即可
git rm –r <dir> # 可以递归删除，即如果后面跟的是一个目录做为参数，则会递归删除整个目录中的所有子目录和文件
git rm –r *  # 进入某个目录中，执行此语句，会删除该目录下的所有文件和子目录
```

<img src="https://raw.githubusercontent.com/Soooooophia/Sooophia_Picgo/master/image-20220721114121188.png" alt="image-20220721114121188" style="zoom:50%;" />

~~~markdown
使用rm命令删除文件后，文件状态变为D，且当前工作区中该文件已被删除
~~~

<img src="https://raw.githubusercontent.com/Soooooophia/Sooophia_Picgo/master/image-20220721114039128.png" alt="image-20220721114039128" style="zoom:50%;" />

~~~markdown
使用rm --cached命令删除文件后，文件会从缓存区被移除，文件状态变为未被追踪，在工作区中仍存在该文件
~~~

### git mv

```shell
git mv [file] [newfile]  # 移动或重命名一个文件、目录或软连接
git mv -f [file] [newfile]  # 如果新文件名已经存在，但还是要重命名它，可以使用 -f 参数
```

<img src="https://raw.githubusercontent.com/Soooooophia/Sooophia_Picgo/master/image-20220721115038786.png" alt="image-20220721115038786" style="zoom:50%;" />

~~~markdown
git mv命令只能修改已经在缓存区中的文件
~~~

<img src="https://raw.githubusercontent.com/Soooooophia/Sooophia_Picgo/master/image-20220721115630532.png" alt="image-20220721115630532" style="zoom:50%;" />

~~~markdown
git mv -f命令修改当前文件为新名称的文件，若新名称已被占用则直接用本文件覆盖原文件
~~~

### git log

~~~shell
git log  # 查看历史提交记录。
git blame <file>  # 以列表形式查看指定文件的历史修改记录
git log --oneline  # 查看历史记录的简洁版本
git log --graph  # graph选项，查看历史中什么时候出现了分支、合并
git log --reverse --oneline  # 逆向查看所有日志
git log --author=<name> --oneline -5  # 查看<name>用户提交的最近5次的记录
git log --oneline --before={3.weeks.ago} --after={2010-04-18} --no-merges  # 要指定日期，可以执行几个选项：--since 和 --before，但是你也可以用 --until 和 --after  【--no-merges 选项以隐藏合并提交】
~~~

### git blame

~~~shell
git blame <file>  # 以列表形式查看指定文件的历史修改记录
~~~

<img src="https://raw.githubusercontent.com/Soooooophia/Sooophia_Picgo/master/image-20220721150141096.png" alt="image-20220721150141096" style="zoom:50%;" />

### git remote

```shell
git remote -v  # 显示所有远程仓库
git remote show [remote]  # 显示某个远程仓库的信息
git remote rm [name]  # 删除远程仓库
git remote rename [old_name] [new_name]  # 修改仓库名
```

### git fetch

~~~shell
git fetch [alias]  # 假设你配置好了一个远程仓库，并且你想要提取更新的数据
# 接着执行
git merge [alias]/[branch]  # 将服务器上的任何更新（假设有人这时候推送到服务器了）合并到你的当前分支
~~~

<img src="https://raw.githubusercontent.com/Soooooophia/Sooophia_Picgo/master/image-20220721170307925.png" alt="image-20220721170307925" style="zoom:50%;" />

~~~markdown
在gitlab远端修改项目中已有的文件，本地仓库先fetch远端仓库后查看对应修改的文件，此时的文件仍然是本地原来的文件并没有实现远端文件的更新，需要进一步执行merge命令将远端文件与本地文件进行合并。而如果不进行fetch直接进行merge操作则会提示以下：
~~~

<img src="https://raw.githubusercontent.com/Soooooophia/Sooophia_Picgo/master/image-20220721170752391.png" alt="image-20220721170752391 " style="zoom:50%;" />

### git pull

git pull 命令用于从远程获取代码并合并本地的版本，git pull其实就是 git fetch和 git merge的简写

```shell
git pull <远程主机名> <远程分支名>:<本地分支名>

git pull origin master:brantest  # 将远程主机 origin 的 master 分支拉取过来，与本地的 brantest 分支合并
git pull origin master  # 如果远程分支是与当前分支合并，则冒号后面的部分可以省略
```

### git push

~~~shell
# 完整命令：
git push <remote_name> <local_branch_name>:<remote_branch_name>  # 写全的命令可以实现本地分支上传远端其他分支
$ git push origin master:refs/for/master  # 将本地的master分支推送到远程主机origin上对应master分支
# 其中，origin 是远程主机名，第一个master是本地分支名，第二个master是远程分支名。
# refs/for 是 gerrit 的规则，意思是提交代码到服务器之后是需要经过code review 之后才能进行merge的，而 refs/heads 不需要。
git push origin --delete master  # 删除主机的分支可以使用--delete参数，该命令表示删除origin主机的master分支
git push --force origin master  # 如果本地版本与远程版本有差异，但又要强制推送可以使用 --force 参数  --慎重使用

# 省略远程分支
git push origin master  # 表示将本地的master分支推送到origin主机的master分支，如果后者不存在，则会被新建
# 省略本地分支
git push origin :master  # 等于推送一个空的本地分支到远程分支，表示删除origin主机的master分支，等同于git push origin --delete master命令
~~~

<img src="https://raw.githubusercontent.com/Soooooophia/Sooophia_Picgo/master/image-20220724220604223.png" alt="image-20220724220604223 " style="zoom:50%;" />

~~~shell
# 尽管上述命令可以实现push命令提交到远程仓库，但此时本地仓库与远程仓库并没有建立追踪关系-->可以查看.git下的config文件查看
# 省略远程和本地分支
git push origin  # 如果当前分支与远程分支存在追踪关系，则本地分支和远程分支都可以省略，将当前分支推送到origin主机的对应分支。使用 git branch -vv 命令来查看本地分支和远程分支的追踪关系。
~~~

<img src="https://raw.githubusercontent.com/Soooooophia/Sooophia_Picgo/master/image-20220724215834065.png" alt="image-20220724215834065" style="zoom:60%;" />

~~~shell
# 全部省略
git push  # 如果当前仓库只有一个远程仓库且本地分支已经与远程分支建立联系，那么主机名也可以省略，自动上传；但是，不管上面delete和上传空分支中哪一种删除远程分支的方式，都不能对本地的相应分支做同时删除动作，如果删除的分支与本地分支存在映射关系，则删除远程分支动作也依然不能删除这个映射关系（即不能修改mapping配置），此时在本地分支直接使用git push依然可以提交到映射关系的对应分支（远程新建）；但是如果使用git branch -d <branchname>则在实现本地仓库的分支删除的同时，解除了该分支与远程仓库的映射关系（修改了config文件）
git push -u origin master | git push --set-upstream origin master  # 如果当前分支与多个主机存在追踪关系，则可以使用-u选项指定一个默认主机，这样后面就可以不加任何参数使用git push
# 注意：本地分支与远程分支的映射只能是1对1的关系，但是二者的名字可以为不同的
~~~

<img src="https://raw.githubusercontent.com/Soooooophia/Sooophia_Picgo/master/image-20220724213835929.png" alt="image-20220724213835929" style="zoom:40%;" />

## 建立远程仓库并连接

~~~markdown
1. 在gitlab上创建一个空的项目并copy项目的地址，例git@github.com:tianqixin/runoob-git-test.git
2. 让本地仓库和远程仓库建立连接
		执行命令git remote add [shortname] [url] //远程仓库地址 --> shortname是远程仓库的名字(如下图1中的git_practice/temp)
		可能报错error:远程 git_practice 已经存在-->使用git remote rm origin命令删除origin远程仓库
3. 拉取远程仓库的文件到本地仓库
		执行命令git pull origin master --allow-unrelated-histories //拉取origin远程仓库中的master分支到本地所在分支(如下图2中的拉取动作)  // allow-unrelated-histories是允许不相关历史提交，强制拉取
4. 提交本地仓库文件到远程仓库
		执行命令git push origin master //提交当前分支内容到origin远程仓库中的master分支(如下图3中的提交动作)
~~~

<img src="https://raw.githubusercontent.com/Soooooophia/Sooophia_Picgo/master/image-20220721152058701.png" alt="image-20220721152058701 " style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/Soooooophia/Sooophia_Picgo/master/image-20220721152752325.png" alt="image-20220721152752325" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/Soooooophia/Sooophia_Picgo/master/image-20220721153151675.png" alt="image-20220721153151675" style="zoom:50%;" />

## Git与远程仓库的token连接

 当需要在本地建立仓库，使用git push命令上传远端仓库时，需要验证用户名和密码，现在git更新后需要token连接

给本地仓库执行以下命令设置远端有权限的仓库

~~~shell
git remote add <remote_name> https://<token>@github.com/<User_name>/<repository>.git

# 例如：
git remote add notes  https://<token>@github.com/Soooooophia/study-notes.git
~~~

连接完远程仓库后，依然需要git push -u origin master | git push --set-upstream origin master命令设置指定本地分支的上游分支，下一次上传直接输入命令git push即可

## Git 分支管理

### git branch

~~~shell
git branch  # 查看当前分支情况及所在位置
git branch newBranch  # 基于当前所在分支位置创建新的分支
git branch -a  # 查看本地和远程的所有分支
git branch -r  # 查看远程分支
git branch -d (branchname)  # 删除分支
git branch -D (branchname)  # 删除尚未完全合并的分支（见下图）
~~~

<img src="https://raw.githubusercontent.com/Soooooophia/Sooophia_Picgo/master/image-20220721172718619.png" alt="image-20220721172718619" style="zoom:50%;" />

本地建立好分支后，需要执行git push --set-upstream <

### git checkout

~~~shell
git checkout branchName  # 切换分支
git checkout -b newBranch  # 创建新分支的同时切换到新的分支
git checkout -b newBranch origin/newBranch  # 在远程项目上拉取本地缺失的分支
git remote update origin --prune ｜ git remote update origin --prune  # 更新远程分支列表
~~~

- git checkout 文件名  # 丢弃对该文件的修改

<img src="https://raw.githubusercontent.com/Soooooophia/Sooophia_Picgo/master/image-20220720190544774.png" alt="image-20220720190544774 " style="zoom:50%;" />

### git merge

~~~shell 
git merge <branchname>  # 将任何分支合并到当前分支中去
~~~

- 合并分支时会存在一定的问题-合并冲突的解决

<img src="https://raw.githubusercontent.com/Soooooophia/Sooophia_Picgo/master/image-20220721173636778.png" alt="image-20220721173636778" />

![image-20220721174148713](https://raw.githubusercontent.com/Soooooophia/Sooophia_Picgo/master/image-20220721174148713.png)

![image-20220721174240868](https://raw.githubusercontent.com/Soooooophia/Sooophia_Picgo/master/image-20220721174240868.png)

### git stash

问题：git pull code的时候遇到you should merge or stash your code, otherwise your code will overwrite

这种情况是， 你本地在coding， 但是远端更新和你本地code 冲突了， 当时此刻想pull remote code时， git不允许你操作，因为在你第一次git pull的时候设置了config git config pull off rebase， 走的是fetch+merge，那么此时你需要将working tree的code stash 到你的local repository

## Git 标签

~~~shell
git tag -a <tag>  # -a选项意为"创建一个带注解的标签"。不用-a选项也可以执行的，但不会记录这标签是啥时候打的，谁打的，也不会让你添加个标签的注解。执行git tag -a 命令时，Git会打开你的编辑器，让你写一句标签注解，就像你给提交写注解一样
git tag -a <tag> <commit>  # 给已经发布的提交追加标签
git tag  # 查看所有标签
~~~

![](https://raw.githubusercontent.com/Soooooophia/Sooophia_Picgo/master/image-20220721175145613-20220722170003163.png)

## 情景使用

### 误删文件

某次删除文件并commit上传至本地仓库后，发现文件需要保留，如何恢复文件

<img src="/Users/songfei/Library/Application Support/typora-user-images/image-20220724212040264.png" alt="image-20220724212040264 " style="zoom:50%;" />

### 代码追加

如果某次commit提交后，发现提交的代码并不完善，想再次修改后提交，但不想创建多次commit的log日志（commit-id），如何实现追加代码

<img src="https://raw.githubusercontent.com/Soooooophia/Sooophia_Picgo/master/image-20220724212431535.png" alt="image-20220724212431535" style="zoom:50%;" />

# Git-flow-2022.7.20

git流程软件 SourceTree

## 前言

### 什么是工作流

代码管理的标准工作流程，企业中如何使用git做版本管理的开发流程

### Git工作流

#### 集中式工作流

如果开发团队成员已经熟悉subversion，该形式工作流可以无需适应全新流程就可以体验git带来的收益，也可以作为向更Git风格工作流迁移的友好过渡

<img src="https://raw.githubusercontent.com/Soooooophia/Sooophia_Picgo/master/image-20220720140833279.png" alt="image-20220720140833279 " style="zoom:35%;" />

##### **优点**

不涉及分支交互操作。

##### **缺点**

- 不适合人员较多的团队，当人员10+时，解决开发人员之间的代码冲突会耗费很多时间。
- master分支提交频繁。
- master分支不稳定，不利于集成测试。

#### 功能分支工作流

以集中式工作流为基础，不同的是为各个新功能分配一个专门的分支来开发，这样可以在把新功能集成到正式项目前，用pull request的方式讨论变更。主要用于比较小的公司

<img src="https://raw.githubusercontent.com/Soooooophia/Sooophia_Picgo/master/image-20220720141115561.png" alt="image-20220720141115561 " style="zoom:50%;" />



#### GitFlow工作流

为功能开发、发布准备和维护分配独立的分支，让发布迭代过程更流畅。严格的分支模型也为大型项目提供了一些非常必要的结构。中大型公司使用该工作流

<img src="https://raw.githubusercontent.com/Soooooophia/Sooophia_Picgo/master/image-20220720141809464.png" alt="image-20220720141809464 " style="zoom:50%;" />

#### Forking工作流

是分布式工作流，充分利用了Git在分支和克隆上的优势。可以安全可靠地管理大团队的开发者（developer），并能接受不信任贡献者（contributor）的提交。

<img src="https://raw.githubusercontent.com/Soooooophia/Sooophia_Picgo/master/image-20220720142141782.png" alt="image-20220720142141782 " style="zoom:50%;" />

### GitFlow工作流

2010年5月，在一篇名为“A successful Git branching model”的博文中，原Git Prime的首席技术官Vincent Driessen介绍了一种构建在Git（一个开源的分布式版本控制系统）之上的软件开发模型。通过利用Git创建和管理分支的能力，为**每个分支设定具有特定的含义名称**，并将软件生命周期中的各类活动**归并到不同的分支**上，实现了**软件开发过程不同阶段的相互隔离**。这种软件开发的活动模型被Vincent称为“Git Flow”（Git工作流程）。Git Flow已经开始流行于基于主干的工作流，它现在被认为是现代连续软件开发和DevOps（开发、技术运营和质量保障三者的交集）实践的最佳实践。

![img](https://upload-images.jianshu.io/upload_images/1366859-eda8da6a7d2385ad.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

- master主干分支：主要分支上存放的是最稳定的正式版本，并且该分支的代码应该是随时可在生产环境中使用的代码。当一个版本开发完毕后，产生了一份新的稳定的可供发布的代码时，主要分支上的代码要被更新。同时，每一次更新，都需要在主要分支上打上对应的版本号。任何人**不允许在主要分支上进行代码的直接提交**，只接受其他分支的合入。原则上主要分支上的代码必须是合并自经过多轮测试及已经发布一段时间且线上稳定的预发分支。
- hotfixes热部署分支：进行主干分支的补丁操作；对于主线功能出现bug的情况，进行补丁操作与主线项目同步进行；当我们在Production发现新的Bug时候，我们需要创建一个Hotfix, 完成Hotfix后，我们**合并回Master和Develop分支**，所以Hotfix的改动会进入下一个Release
- release预部署分支：测试工程师调用的分支，当你需要一个发布一个新Release的时候，我们基于Develop分支创建一个Release分支，完成Release后，我们合并到Master和Develop分支
- develop开发分支：开发源代码分支，主开发分支，包含所有要发布到下一个Release的代码，这个主要合并与其他分支，比如Feature分支
- feature功能分支：功能开发的分支，也就是开发人员使用的分支

![image-20220721163055958](https://raw.githubusercontent.com/Soooooophia/Sooophia_Picgo/master/image-20220721163055958.png)

~~~markdown
以上是一个gitflow流程管理的实例. 在项目开始前，架构师先创建一个git仓库，包含了master/main分支，发布一个可运行的基础项目架构；在该基础上创建develop分支，并发布关于此次开发分支主要完成的功能和要求等；接着项目人员在develop分支上继续创建feature分支（多个功能多个分支），分别进行功能的开发实现，待研究人员开发完成后，申请合并到develop分支；测试人员会基于develop分支继续创建release分支进行功能测试，若测试中出现bug等问题则将问题反馈给develop分支，继续进行功能修补，若测试无问题则申请与master/main分支合并，作为可运行开发版本上线运行；若在项目使用过程中出现bug需要修复，则基于该问题项目版本创建hotfixes分支进行补丁修复开发，完成后需要同时合并到develop分支和master/main分支，实现对项目的完善。
~~~
