
- [分支](#branch)     
  - [分支创建](#branch_add)     
  - [分支切换](#branch_checkout)    
  - [分支删除](#branch_delete)       
  - [分支查看](#branch_log)     
  - [分支比较](#branch_diff)     
  - [分支跟踪](#branch_track)     
  - [创建远程分支](#branch_remoteadd)     
  - [删除远程分支](#branch_remotedelete)        
  - [修改分支名称](#branch_rename)     
  

## <a id="branch">分支<a/>

Git 保存的不是文件的变化或者差异，而是一系列不同时刻的文件快照。在进行提交操作时，Git 会保存一个提交对象（commit object）。
  
Git 的分支，其实本质上仅仅是指向提交对象的可变指针。 Git 的默认分支名字是 master。 在多次提交操作之后，你其实已经有一个指向最后那个提交对象的 master 分支。 它会在每次的提交操作中自动向前移动。Git 的 “master” 分支并不是一个特殊分支。 它就跟其它分支完全没有区别。 之所以几乎每一个仓库都有 master 分支，是因为 git init 命令默认创建它，并且大多数人都懒得去改动它。

![Git theory](../images/branch-and-history.png)    
图：Git 分支及其提交历史

### <a id="branch_add">分支创建</a>
创建分支命令

    $ git branch testing
git branch 命令仅仅创建一个新分支，并不会自动切换到新分支中去。

* 如何实现新建一个分支并同时切换到那个分支上？      

运行一个带有 -b 参数的 git checkout 命令

    $ git checkout -b iss53
它是下面两条命令的简写：

    $ git branch iss53
    $ git checkout iss53

* 根据历史提交创建分支。    

  根据 hash 创建:

      git branch branchname <sha1-of-commit>

  根据引用创建:

      git branch branchname HEAD~3

  根据 hash 或引用创建后同时切换:

      git checkout -b branchname <sha1-of-commit or HEAD~3>

### <a id="branch_checkout">分支切换</a>
分支切换会改变你工作目录中的文件。在切换分支时，一定要注意你工作目录里的文件会被改变。 如果是切换到一个较旧的分支，你的工作目录会恢复到该分支最后一次提交时的样子。 如果 Git 不能干净利落地完成这个任务，它将禁止切换分支。

    git checkout testing

### <a id="branch_delete">分支删除</a>

    git branch -d testing
        
### <a id="branch_log">分支查看</a>
◎ `git branch` 不加任何参数，查看分支类表

    $ git branch
      iss53
    * master
      testing
注意 master 分支前的 * 字符：它代表现在检出的那一个分支（也就是说，当前 HEAD 指针所指向的分支）。 这意味着如果在这时候提交，master 分支将会随着新的工作向前移动。

如果需要查看每一个分支的最后一次提交，可以运行 `git branch -v` 命令：

    $ git branch -v
      iss53   93b412c fix javascript issue
    * master  7a98805 Merge branch 'iss53'
      testing 782fd34 add scott to the author list in the readmes

`--merged` 与 `--no-merged` 这两个有用的选项可以过滤这个列表中已经合并或尚未合并到当前分支的分支。 如果要查看哪些分支已经合并到当前分支，可以运行 ：

    $ git branch --merged
      iss53
    * master

查看所有包含未合并工作的分支，可以运行 git branch --no-merged：

    $ git branch --no-merged
      testing
  
◎ 查看分支也可以使用 `git log` 命令，该命令有许多选项， 详细使用方法见官方文档。

* 你可以简单地使用 git log 命令查看各个分支当前所指的对象。提供这一功能的参数是 --decorate。
```
    git log --oneline --decorate
```    
该命令是单行及sha1缩写的方式显示。

* 输出你的提交历史、各个分支的指向以及项目的分支分叉情况
```
    $ git log --oneline --decorate --graph --all
    * c2b9e (HEAD, master) made other changes
    | * 87ab2 (testing) made a change
    |/
    * f30ab add feature #32 - ability to add new formats to the
    * 34ac2 fixed bug #1328 - stack overflow under certain conditions
    * 98ca9 initial commit of my project
```
* 加上 pretty 参数显示     
格式： `git log --pretty[=<format>]` 。其中，format 可以是预设的值，也可以是自定义值。 

1.预设的值包括 `oneline`, `short`, `medium`, `full`, `fuller` 等等。例如：

    $ git log --pretty=fuller
    显示格式：
    commit <sha1>
    Author:     <author>
    AuthorDate: <author date>
    Commit:     <committer>
    CommitDate: <committer date>
    <title line>
    <full commit message>

2.自定义的格式为： `format:<string>`。例如：

    $ git log --pretty=format:"%h [[%s]] %cd"
    6bc023d [[modify material tech check]] Wed Jan 16 20:58:57 2019 +0800


* 配置 git lg 可以看到彩色的日志     
```
    $ git config --global alias.lg "log --color --graph 
    --pretty=format:'%Cred%h%Creset 
    -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

### <a id="branch_diff">分支比较</a>
分支比较可以使用 `git diff` 命令，也可以使用 `git log` 命令，这两条命令的侧重点不同。

##### 使用 `git diff` 比较文件的差异

###### 显示出所有有差异的文件的详细差异

    git diff branch1 branch2

如果差异比较多，看起来不是很方便，可以将差异输出到文件中：

    git diff [branchA] [branchB] >>d:/file.diff

###### 显示指定文件的详细差异

    git diff branch1 branch2 文件名(带路径) 

###### 显示出所有有差异的文件列表

    git diff branch1 branch2 --stat

> ※ 查看同一分支的不同提交的差异也类似。

##### 使用 `git log` 比较提交的差异

###### 查看一个分支有，另一个分支没有

    git log branch1 ^branch2  查看 branch1 有，而 branch2 中没有
    git log branch2 ^branch1  查看 branch2 有，而 branch1 中没有

###### 查看一个分支比另一个分支多了哪些内容

    git log branch1..branch2  查看 branch2 中比 branch1 中多提交了哪些内容
    git log branch2..branch1  查看 branch1 中比 branch2 中多提交了哪些内容

###### 查看所有不同

    git log branch1...branch2
加上 `–left-right` 后，左箭头 < 表示是 branch1 的，右箭头 > 表示是 branch2 的。

    git log --left-right dev...master


### <a id="branch_track">跟踪分支</a>       
* 从一个远程跟踪分支检出一个本地分支会自动创建一个叫做 “跟踪分支”（有时候也叫做 “上游分支”）。Git 提供了 --track 快捷方式：
```
    git checkout --track origin/serverfix
```
* 如果想要将本地分支与远程分支设置为不同名字，你可以轻松地增加一个不同名字的本地分支的上一个命令：
```
    git checkout -b sf origin/serverfix
```
* 设置已有的本地分支跟踪一个刚刚拉取下来的远程分支，或者想要修改正在跟踪的上游分支
```
    git branch -u origin/serverfix
```
* 查看设置的所有跟踪分支
```
    $ git branch -vv
      iss53     7e424c3 [origin/iss53: ahead 2] forgot the brackets
      master    1ae2a45 [origin/master] deploying index fix
    * serverfix f8674d9 [teamone/server-fix-good: ahead 3, behind 1] this should do it
      testing   5ea463a trying something new
```      
这里可以看到：      
1. iss53 分支正在跟踪 origin/iss53 并且 “ahead” 是 2，意味着本地有两个提交还没有推送到服务器上。 
2. master 分支正在跟踪 origin/master 分支并且是最新的。 
3. serverfix 分支正在跟踪 teamone 服务器上的 server-fix-good 分支并且领先 3 落后 1，意味着服务器上有一次提交还没有合并入同时本地有三次提交还没有推送。 
4. testing 分支并没有跟踪任何远程分支。

> 需要重点注意的一点是这些数字的值来自于你从每个服务器上最后一次抓取的数据。 这个命令并没有连接服务器，它只会告诉你关于本地缓存的服务器数据。 如果想要统计最新的领先与落后数字，需要在运行此命令前抓取所有的远程仓库。 可以像这样做：```$ git fetch --all; git branch -vv```    


### <a id="branch_remoteadd">创建远程分支</a>
`git push origin master` 命令在没有track远程分支的本地分支中默认提交的master分支，因为master分支默认指向了origin master 分支。

如果想把本地的某个分支提交到远程仓库，并作为远程仓库的master分支，或者作为另外一个名叫test的分支，那么可以这么做。

    $ git push origin test:master         // 提交本地test分支作为远程的master分支
    $ git push origin test:test           // 提交本地test分支作为远程的test分支
 
### <a id="branch_remotedelete">删除远程分支</a>
从服务器上删除 serverfix 分支，运行下面的命令：

    $ git push origin --delete serverfix
    To https://github.com/schacon/simplegit
     - [deleted]         serverfix 
或者：     
如果:左边的分支为空，那么将删除:右边的远程的分支。

    $ git push origin :test              // 刚提交到远程的test将被删除，但是本地还会保存的
> 有待验证?

    git push origin :branch-name

冒号前面的空格不能少，原理是把一个空分支push到server上，相当于删除该分支。

### <a id="branch_rename">修改分支名称</a>
官方使用帮助中说明：         
<cite>
With a -m or -M option, <oldbranch> will be renamed to <newbranch>. If <oldbranch> had a corresponding reflog, it is renamed to match <newbranch>, and a reflog entry is created to remember the branch renaming. If <newbranch> exists, -M must be used to force the rename to happen.
</cite>

本地分支重命名

    git branch -m oldName newName

远程分支重命名

    git branch -m oldName newName   重命名远程分支对应的本地分支
    git push --delete origin oldName    删除远程分支
    git push origin newName 上传新命名的本地分支
    git branch --set-upstream-to origin/newName 把修改后的本地分支与远程分支关联

***

## 参考资料    
[分支开发工作流](https://git-scm.com/book/zh/v2/Git-分支-分支开发工作流)      
[远程分支](https://git-scm.com/book/zh/v2/Git-分支-远程分支)     



