- [git 命令](#git_command)         
  - [初始化](#git_init)  
  - [克隆](#git_clone)     
  - [检查状态](#git_status)    
  - [跟踪新文件](#git_add)      
  - [提交更新](#git_commit)     
  - [移除文件](#git_remove)     
  - [移动文件](#git_move)      
  - [查看提交历史](#git_log)       
  - [撤消操作](#git_cancle)    
  - [取消暂存的文件](#git_reset)     
  - [撤消对文件的修改/切换分支](#git_checkout)     

## <a id="git_command">git 命令<a/>

### <a id="git_init">初始化<a/>
  
`git init`: 在现有目录中初始化仓库  

如果你打算使用 Git 来对现有的项目进行管理，你只需要进入该项目目录并输入：

    $ git init

该命令将创建一个名为 .git 的子目录，这个子目录含有你初始化的 Git 仓库中所有的必须文件，这些文件是 Git 仓库的骨干。 但是，在这个时候，我们仅仅是做了一个初始化的操作，你的项目里的文件还没有被跟踪。通过 git add 命令来实现对指定文件的跟踪，然后执行 git commit 提交：  

    $ git add *.c
    $ git add LICENSE
    $ git commit -m 'initial project version'
***

### <a id="git_clone">克隆<a/>
  
`git clone`: 克隆现有的仓库

Git 克隆的是该 Git 仓库服务器上的几乎所有数据，而不是仅仅复制完成你的工作所需要文件。
命令格式：

    git clone [url]
  
例如：

    $ git clone https://github.com/libgit2/libgit2  
    
克隆远程仓库的时候，自定义本地仓库的名字

    $ git clone https://github.com/libgit2/libgit2 mylibgit

depth 用于指定克隆深度，为1即表示只克隆最近一次commit。 

    git clone --depth=1
    
克隆指定分支的代码

    git clone -b 分支名 仓库地址

***

### <a id="git_status">检查状态<a/>
  
`git status`: 检查当前文件状态

git status 命令的输出十分详细，但其用语有些繁琐。 

    $ git status
    On branch master
    nothing to commit, working directory clean

如果你使用 git status -s 命令或 git status --short 命令，你将得到一种更为紧凑的格式输出。
  
    $ git status -s
    M README
    MM Rakefile
    A  lib/git.rb
    M  lib/simplegit.rb
    ?? LICENSE.txt

* 新添加的未跟踪文件前面有 ?? 标记    
* 新添加到暂存区中的文件前面有 A 标记   
* 改过的文件前面有 M 标记。    
  
> 你可能注意到了 M 有两个可以出现的位置，出现在右边的 M 表示该文件被修改了但是还没放入暂存区，出现在靠左边的 M 表示该文件被修改了并放入了暂存区。更多状态请[查看](./git-status.md)。
***

###  <a id="git_add">跟踪新文件<a/>
  
`git add`: 跟踪新文件

使用命令 git add 开始跟踪一个文件。 

    $ git add README

跟踪多个文件  

    $ git add .
    
此时再运行 git status 命令，会看到 README 文件已被跟踪，并处于暂存状态：

    $ git status
    On branch master
    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)

        new file:   README

只要在 Changes to be committed 这行下面的，就说明是已暂存状态。

***

### <a id="git_commit">提交更新<a/>

`git commit`: 提交更新

在暂存区域已经准备妥当可以提交了。 在此之前，请一定要确认还有什么修改过的或新建的文件还没有 git add 过，否则提交的时候不会记录这些还没暂存起来的变化。 这些修改过的文件只保留在本地磁盘。 所以，每次准备提交前，先用 git status 看下，是不是都已暂存起来了， 然后再运行提交命令 git commit：

    $ git commit

这种方式会启动文本编辑器以便输入本次提交的说明。
另外，你也可以在 commit 命令后添加 -m 选项，将提交信息与命令放在同一行：

    $ git commit -m "Story 182: Fix benchmarks for speed"

跳过使用暂存区域
git commit 加上 -a 选项，Git 就会自动把所有已经跟踪过的文件暂存起来一并提交，从而跳过 git add 步骤。
  
    $ git commit -a -m 'added new benchmarks'
    [master 83e38c7] added new benchmarks
     1 file changed, 5 insertions(+), 0 deletions(-)
***

### <a id="git_remove">移除文件<a/>
  
`git rm`: 移除文件

要从 Git 中移除某个文件，就必须要从已跟踪文件清单中移除（确切地说，是从暂存区域移除），然后提交。
如果只是简单地从工作目录中手工删除文件，运行 git status 时就会在 “Changes not staged for commit” 部分（也就是 未暂存清单）看到：

    $ rm PROJECTS.md
    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Changes not staged for commit:
      (use "git add/rm <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)

            deleted:    PROJECTS.md

    no changes added to commit (use "git add" and/or "git commit -a")
然后再运行 git rm 记录此次移除文件的操作：

    $ git rm PROJECTS.md
    rm 'PROJECTS.md'
    $ git status
    On branch master
    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)

        deleted:    PROJECTS.md
下一次提交时，该文件就不再纳入版本管理了。 如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 -f。 这是一种安全特性，用于防止误删还没有添加到快照的数据，这样的数据不能被 Git 恢复。

另外一种情况是，我们想把文件从 Git 仓库中删除（亦即从暂存区域移除），但仍然希望保留在当前工作目录中。 换句话说，你想让文件保留在磁盘，但是并不想让 Git 继续跟踪。 当你忘记添加 .gitignore 文件，不小心把一个很大的日志文件或一堆 .a 这样的编译生成文件添加到暂存区时，这一做法尤其有用。 为达到这一目的，使用 --cached 选项：

    $ git rm --cached README
git rm 命令后面可以列出文件或者目录的名字，也可以使用 glob 模式。 比方说：

    $ git rm log/\*.log
注意到星号 * 之前的反斜杠 \， 因为 Git 有它自己的文件模式扩展匹配方式，所以我们不用 shell 来帮忙展开。 此命令删除 log/ 目录下扩展名为 .log 的所有文件。 类似的比如：

    $ git rm \*~
该命令为删除以 ~ 结尾的所有文件。
***

### <a id="git_move">移动文件<a/>

`git mv`: 移动文件

    $ git mv file_from file_to
它会恰如预期般正常工作。 
其实，运行 git mv 就相当于运行了下面三条命令：

    $ mv README.md README
    $ git rm README.md
    $ git add README
***

### <a id="git_log">查看提交历史<a/>
`git log` 查看提交历史  

    $ git log
    commit ca82a6dff817ec66f44342007202690a93763949
    Author: Scott Chacon <schacon@gee-mail.com>
    Date:   Mon Mar 17 21:52:11 2008 -0700

        changed the version number
    ......

一个常用的选项是 -p，用来显示每次提交的内容差异。 你也可以加上 -2 来仅显示最近两次提交：

    $ git log -p -2
    ......

`--oneline` 标记将每个commit压缩成一行，-n 来限制输出的数量

    $ git long --oneline -n 8
    717c97a (HEAD -> master, upstream/master) Merge pull request #11 from Smallbucket/master
    d3ab992 Update README.md
    8b78f95 Create clone-last.md
    dd6c553 Update README.md
    29a7e56 (origin/master, origin/HEAD) Merge pull request #10 from Smallbucket/master
    8ed1ed4 Update README.md
    5e8a847 Create git-status.md
    c4ef2f7 Merge pull request #9 from Smallbucket/master

更多查看[格式化log输出](https://www.cnblogs.com/irocker/p/advanced-git-log.html)         
***

### <a id="git_cancel">撤消操作<a/>

    $ git commit --amend
这个命令会将暂存区中的文件提交。 如果自上次提交以来你还未做任何修改（例如，在上次提交后马上执行了此命令），那么快照会保持不变，而你所修改的只是提交信息。

文本编辑器启动后，可以看到之前的提交信息。 编辑后保存会覆盖原来的提交信息。

例如，你提交后发现忘记了暂存某些需要的修改，可以像下面这样操作：

    $ git commit -m 'initial commit'
    $ git add forgotten_file
    $ git commit --amend
最终你只会有一个提交 - 第二次提交将代替第一次提交的结果。
***

### <a id="git_reset">取消暂存的文件<a/>
`git reset`: 取消暂存的文件

    git reset HEAD <file>...
例如，你已经修改了两个文件并且想要将它们作为两次独立的修改提交，但是却意外地输入了 git add * 暂存了它们两个。 如何只取消暂存两个中的一个呢？

    $ git reset HEAD CONTRIBUTING.md
    Unstaged changes after reset:
    M CONTRIBUTING.md
    $ git status
    On branch master
    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)

        renamed:    README.md -> README

    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   CONTRIBUTING.md
多个文件需要加上 --

    git reset HEAD -- public/libs/*
    
记住，任何已经提交到 Git 的都可以被恢复。即便在已经删除的分支中的提交，或者用 --amend 重新改写的提交，都可以被恢复。

取消上一次 commit
  
    git reset --hard HEAD~1
    
取消上一次 commit(保证你修改)

    git reset --soft HEAD~1   
***

### <a id="git_checkout">撤消对文件的修改/切换分支<a/>
`git checkout`: 撤消对文件的修改
你该如何方便地撤消修改 - 将它还原成上次提交时的样子（或者刚克隆完的样子，或者刚把它放入工作目录时的样子）？

    $ git checkout -- CONTRIBUTING.md
    $ git status
    On branch master
    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)

        renamed:    README.md -> README
可以看到那些修改已经被撤消了。
  
