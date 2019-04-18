
[git 命令](#git_command)    
  - [初始化](#git_init)  
  - [克隆](#git_clone)     
  - [状态](#git_status)   
  - [远程仓库回滚](#git_rollback)
  
***

## <a id="git_command">git 命令<a/>

### <a id="git_init">git init 在现有目录中初始化仓库<a/>

如果你打算使用 Git 来对现有的项目进行管理，你只需要进入该项目目录并输入：

    $ git init

该命令将创建一个名为 .git 的子目录，这个子目录含有你初始化的 Git 仓库中所有的必须文件，这些文件是 Git 仓库的骨干。 但是，在这个时候，我们仅仅是做了一个初始化的操作，你的项目里的文件还没有被跟踪。通过 git add 命令来实现对指定文件的跟踪，然后执行 git commit 提交：  

    $ git add *.c
    $ git add LICENSE
    $ git commit -m 'initial project version'
***

### <a id="git_clone">git clone 克隆现有的仓库<a/>

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

### <a id="git_status">git status 检查当前文件状态<a/>

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
  
> 你可能注意到了 M 有两个可以出现的位置，出现在右边的 M 表示该文件被修改了但是还没放入暂存区，出现在靠左边的 M 表示该文件被修改了并放入了暂存区。
***

###  <a id="git_status">git add 跟踪新文件<a/>

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

### <a id="git_status">忽略文件<a/>
一般我们总会有些文件无需纳入 Git 的管理，也不希望它们总出现在未跟踪文件列表。 通常都是些自动生成的文件，比如日志文件，或者编译过程中创建的临时文件
等。 在这种情况下，我们可以创建一个名为 .gitignore 的文件，列出要忽略的文件模式。

    $ cat .gitignore
    *.[oa]
    *~

文件 .gitignore 的格式规范如下：

    * 所有空行或者以 ＃ 开头的行都会被 Git 忽略。
    * 可以使用标准的 glob 模式匹配。
    * 匹配模式可以以（/）开头防止递归。
    * 匹配模式可以以（/）结尾指定目录。
    * 要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（!）取反。
***

### <a id="git_status">git commit 提交更新<a/>

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

### <a id="git_status">git rm 移除文件<a/>

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

### <a id="git_status">git mv 移动文件<a/>

    $ git mv file_from file_to
它会恰如预期般正常工作。 
其实，运行 git mv 就相当于运行了下面三条命令：

    $ mv README.md README
    $ git rm README.md
    $ git add README
***

### <a id="git_status">git log 查看提交历史<a/>

    $ git log
    commit ca82a6dff817ec66f44342007202690a93763949
    Author: Scott Chacon <schacon@gee-mail.com>
    Date:   Mon Mar 17 21:52:11 2008 -0700

        changed the version number
    ......

一个常用的选项是 -p，用来显示每次提交的内容差异。 你也可以加上 -2 来仅显示最近两次提交：

    $ git log -p -2
    ......
***

### <a id="git_status">撤消操作<a/>

    $ git commit --amend
这个命令会将暂存区中的文件提交。 如果自上次提交以来你还未做任何修改（例如，在上次提交后马上执行了此命令），那么快照会保持不变，而你所修改的只是提交信息。

文本编辑器启动后，可以看到之前的提交信息。 编辑后保存会覆盖原来的提交信息。

例如，你提交后发现忘记了暂存某些需要的修改，可以像下面这样操作：

    $ git commit -m 'initial commit'
    $ git add forgotten_file
    $ git commit --amend
最终你只会有一个提交 - 第二次提交将代替第一次提交的结果。
***

### <a id="git_status">git reset 取消暂存的文件<a/>

git reset HEAD <file>...
例如，你已经修改了两个文件并且想要将它们作为两次独立的修改提交，但是却意外地输入了 git add * 暂存了它们两个。 如何只取消暂存两个中的一个呢？

    $ git reset HEAD CONTRIBUTING.md
    Unstaged changes after reset:
    M	CONTRIBUTING.md
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

### <a id="git_status">git checkout 撤消对文件的修改<a/>
你该如何方便地撤消修改 - 将它还原成上次提交时的样子（或者刚克隆完的样子，或者刚把它放入工作目录时的样子）？

    $ git checkout -- CONTRIBUTING.md
    $ git status
    On branch master
    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)

        renamed:    README.md -> README
可以看到那些修改已经被撤消了。
***

### <a id="git_status">git remote 查看远程仓库<a/>

    $ git remote
    origin
你也可以指定选项 -v，会显示需要读写远程仓库使用的 Git 保存的简写与其对应的 URL。

    $ git remote -v
    origin	https://github.com/schacon/ticgit (fetch)
    origin	https://github.com/schacon/ticgit (push)
***

### <a id="git_status">git remote add 添加远程仓库<a/>
在本地的git仓库"添加一个远程仓库",当然这个远程仓库还是你自己的这个目录。
$ git remote add origin ssh://你的IP/~/testgit/.git
    
    $ git remote
    origin
    $ git remote add pb https://github.com/paulboone/ticgit
    $ git remote -v
    origin	https://github.com/schacon/ticgit (fetch)
    origin	https://github.com/schacon/ticgit (push)
    pb	https://github.com/paulboone/ticgit (fetch)
    pb	https://github.com/paulboone/ticgit (push)
添加一个新的远程 Git 仓库，同时指定一个你可以轻松引用的简写。
***

### <a id="git_status">git fetch 从远程仓库中拉取<a/>
这个命令会访问远程仓库，从中拉取所有你还没有的数据。 执行完成后，你将会拥有那个远程仓库中所有分支的引用，可以随时合并或查看。

    $ git fetch [remote-name]
git fetch origin 会抓取克隆（或上一次抓取）后新推送的所有工作。 必须注意 git fetch 命令会将数据拉取到你的本地仓库 - 它并不会自动合并或修改你当前的工作。 当准备好时你必须手动将其合并入你的工作。
***

### <a id="git_status">git pull 从远程仓库中抓取与拉取<a/>
自动的抓取然后合并远程分支到当前分支。 

    $ git pull [remote-name]
***

### <a id="git_status">git push 推送到远程仓库<a/>

    $ git push origin master
当你和其他人在同一时间克隆，他们先推送到上游然后你再推送到上游，你的推送就会毫无疑问地被拒绝。 你必须先将他们的工作拉取下来并将其合并进你的工作后才能推送。
***

### <a id="git_status">git remote show 查看远程仓库详细信息<a/>

$ git remote show origin
* remote origin
  Fetch URL: https://github.com/schacon/ticgit
  Push  URL: https://github.com/schacon/ticgit
  HEAD branch: master
  Remote branches:
    master                               tracked
    dev-branch                           tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)
***

### <a id="git_status">git remote rename 远程仓库的重命名<a/>

    $ git remote rename pb paul
    $ git remote
    origin
    paul
***

### <a id="git_status">git remote rm 远程仓库的移除<a/>

    $ git remote rm paul
    $ git remote
    origin
***

### <a id="git_status">git remote set-url 更新获取地址方式<a/>
> set-url 有两个参数：当前远程库名字、为远程库新设置的地址

    git remote set-url origin http://git.example.cn:3000/project/ptest.git
***

### <a id="git_status">git stash 储藏你的工作<a/>
为了演示这一功能，你可以进入你的项目，在一些文件上进行工作，有可能还暂存其中一个变更。如果你运行 git status，你可以看到你的中间状态：

    $ git status
     On branch master
     Changes to be committed:
       (use "git reset HEAD <file>..." to unstage)

          modified:   index.html

     Changes not staged for commit:
       (use "git add <file>..." to update what will be committed)

          modified:   lib/simplegit.rb


现在你想切换分支，但是你还不想提交你正在进行中的工作；所以你储藏这些变更。为了往堆栈推送一个新的储藏，只要运行 git stash：

    $ git stash
    Saved working directory and index state \
      "WIP on master: 049d078 added the index file"
    HEAD is now at 049d078 added the index file
    (To restore them type "git stash apply")
你的工作目录就干净了：

    $ git status
    # On branch master
    nothing to commit, working directory clean
    git pull
    git stash pop
***

### <a id="git_status">git reflog 显示整个本地仓储的 commit<a/>
显示整个本地仓储的commit, 包括所有branch的commit, 甚至包括已经撤销的commit, 只要HEAD发生了变化, 就会在reflog里面看得到. git log只包括当前分支的commit。
例如：

    $ git reflog
    b7057a9 HEAD@{0}: reset: moving to b7057a9
    98abc5a HEAD@{1}: commit: more stuff added to foo
    b7057a9 HEAD@{2}: commit (initial): initial commit

所以，我们要找回我们第二commit，只需要做如下操作：
    
    $ git reset --hard 98abc5a

再来看一下 git 记录：

    $ git log
    * 98abc5a (HEAD, master) more stuff added to foo
    * b7057a9 initial commit

所以，如果你因为reset等操作丢失一个提交的时候，你总是可以把它找回来。除非你的操作已经被git当做垃圾处理掉了，一般是30天以后。
***

### <a id="git_status">git blame<a/> 
命令模式会显示所有行的信息。我们可以利用"<开始>，<结束>"表示，<开始>和<结束>参数可以是数字。

    $ git blame -L 12,13 hello.html 
将显示第12、13行代码的信息
<结束>这个参数不必非得指定一个数字，可以使用+N或者-N指定范围。
    
    $ git blame -L 12,-3 hello.html
也可以使用正则表达式进行匹配行内容，和已经查询以往某个版本中的内容。如：

    $ git blame -L "/<\/body>/",-2 4333289e^ -- index.html
注意上面有--，这是通知Git查询指定文件。

***

### <a id="git_status">git diff<a/>
[git diff](http://blog.csdn.net/csfreebird/article/details/8044796) 可以比较working tree同index之间，index和git directory之间，working tree和git directory之间，git directory中不同commit之间的差异。

***

### <a id="git_status">git branch<a/>

* 创建分支

        git branch testing

* 切换分支

        git checkout testing

* 新建一个分支并同时切换到那个分支上
运行一个带有 -b 参数的 git checkout 命令

        git checkout -b iss53
        
* 你可以简单地使用 git log 命令查看各个分支当前所指的对象。 提供这一功能的参数是 --decorate。

        git log --oneline --decorate

* 输出你的提交历史、各个分支的指向以及项目的分支分叉情况

        git log --oneline --decorate --graph --all

* 配置 git lg 可以看到彩色的日志
    
    git config --global alias.lg "log --color --graph 
    --pretty=format:'%Cred%h%Creset 
    -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

* 删除分支

        git branch -d testing
    
* 跟踪分支       
从一个远程跟踪分支检出一个本地分支会自动创建一个叫做 “跟踪分支”（有时候也叫做 “上游分支”）。

Git 提供了 --track 快捷方式：

    git checkout --track origin/serverfix

如果想要将本地分支与远程分支设置为不同名字，你可以轻松地增加一个不同名字的本地分支的上一个命令：

    git checkout -b sf origin/serverfix

设置已有的本地分支跟踪一个刚刚拉取下来的远程分支，或者想要修改正在跟踪的上游分支

    git branch -u origin/serverfix
 
查看设置的所有跟踪分支

    git branch 的 -vv 
例如：

    $ git branch -vv
      iss53     7e424c3 [origin/iss53: ahead 2] forgot the brackets
      master    1ae2a45 [origin/master] deploying index fix
    * serverfix f8674d9 [teamone/server-fix-good: ahead 3, behind 1] this should do it
      testing   5ea463a trying something new
这里可以看到：      
1. iss53 分支正在跟踪 origin/iss53 并且 “ahead” 是 2，意味着本地有两个提交还没有推送到服务器上。 
2. master 分支正在跟踪 origin/master 分支并且是最新的。 
3. serverfix 分支正在跟踪 teamone 服务器上的 server-fix-good 分支并且领先 3 落后 1，意味着服务器上有一次提交还没有合并入同时本地有三次提交还没有推送。 
4. testing 分支并没有跟踪任何远程分支。

> 需要重点注意的一点是这些数字的值来自于你从每个服务器上最后一次抓取的数据。 这个命令并没有连接服务器，它只会告诉你关于本地缓存的服务器数据。 如果想要统计最新的领先与落后数字，需要在运行此命令前抓取所有的远程仓库。 可以像这样做：

    $ git fetch --all; git branch -vv    
 
* 删除远程分支        
从服务器上删除 serverfix 分支，运行下面的命令：

    $ git push origin --delete serverfix
    To https://github.com/schacon/simplegit
     - [deleted]         serverfix 
或者：     
如果:左边的分支为空，那么将删除:右边的远程的分支。

    $ git push origin :test              // 刚提交到远程的test将被删除，但是本地还会保存的
> 有待验证?


* 创建远程分支      
git push origin master 命令在没有track远程分支的本地分支中默认提交的master分支，因为master分支默认指向了origin master 分支。

如果想把本地的某个分支提交到远程仓库，并作为远程仓库的master分支，或者作为另外一个名叫test的分支，那么可以这么做。

    $ git push origin test:master         // 提交本地test分支作为远程的master分支
    $ git push origin test:test           // 提交本地test分支作为远程的test分支


***

### <a id="git_status">存储用户名与密码<a/>
保存在内存中：

    git config --global credential.helper cache
    which tells git to keep your password cached in memory for (by default) 15 minutes. You can set a longer timeout with:
    git config --global credential.helper "cache --timeout=3600"

保存在磁盘上：

    git config --global credential.helper store

***

### <a id="git_rollback">远程仓库回滚<a/>

#### 删除最后一次提交 git revert


    git revert HEAD
    git push origin master
> revert是放弃指定提交的修改，但是会生成一次新的提交，需要填写提交注释，以前的历史记录都在; 而reset是指将HEAD指针指到指定提交，历史记录中不会出现放弃的提交记录。

#### 删除历史某次提交(git rebase) 
这种情况需要先用git log命令在历史记录中查找到想要删除的某次提交的commit id， 然后执行以下命令：

    git rebase -i "commit id"^
> "commit id"替换为想要删除的提交的"commit id"，需要注意最后的^号，意思是commit id的前一次提交。
命令:

    git rebase -i  [startpoint]  [endpoint]
其中-i的意思是--interactive，即弹出交互式的界面让用户编辑完成合并操作，[startpoint]  [endpoint]则指定了一个编辑区间，如果不指定[endpoint]，则该区间的终点默认是当前分支HEAD所指向的commit(注：该区间指定的是一个前开后闭的区间)。

执行该条命令之后会打开一个编辑框，列出了包含该次提交在内之后的所有提交:

```none
pick 3c45746 3 commit
pick 925e14a 4 commit

# Rebase d9ecfb6..925e14a onto d9ecfb6 (2 commands)
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
# d, drop = remove commit
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```
上面是指令编辑区， 中间是指令说明。
* pick：保留该commit（缩写:p）
* reword：保留该commit，但我需要修改该commit的注释（缩写:r）
* edit：保留该commit, 但我要停下来修改该提交(不仅仅修改注释)（缩写:e）
* squash：将该commit和前一个commit合并（缩写:s）
* fixup：将该commit和前一个commit合并，但我不要保留该提交的注释信息（缩写:f）
* exec：执行shell命令（缩写:x）
* drop：我要丢弃该commit（缩写:d）

然后在编辑框中删除你想要删除的提交所在行，然后保存退出就好啦，如果有冲突的需要解决冲突。接下来，执行以下命令，将本地仓库提交到远程库就完成了：

    git push origin master -f
    
#### 修改历史某次提交(git rebase) 
这种情况的解决方法类似于删除历史某次提交，只需要在第二条打开编辑框之后，将你想要修改的提交所在行的pick替换成edit然后保存退出，这个时候rebase会停在你要修改的提交，然后做你需要的修改，修改完毕之后，执行以下命令：

    git add .
    git commit --amend
    git rebase --continue
如果你在之前的编辑框修改了n行，也就是说要对n次提交做修改，则需要重复执行以上步骤n次。

> 需要注意的是，在执行rebase命令对指定提交修改或删除之后，该次提交之后的所有提交的"commit id"都会改变。



***

### git 标签
#### 查看本地 tag

    git tag

#### 创建标签   
Git 使用两种主要类型的标签：轻量标签（lightweight）与附注标签（annotated）。     

* 附注标签     
在 Git 中创建一个附注标签是很简单的。 最简单的方式是当你在运行 tag 命令时指定 -a 选项：

      $ git tag -a v1.4 -m 'my version 1.4'
      $ git tag
      v0.1
      v1.3
      v1.4

* 轻量标签    
另一种给提交打标签的方式是使用轻量标签。 轻量标签本质上是将提交校验和存储到一个文件中 - 没有保存任何其他信息。 创建轻量标签，不需要使用 -a、-s 或 -m 选项，只需要提供标签名字：

      $ git tag v1.4-lw
      $ git tag
      v0.1
      v1.3
      v1.4
      v1.4-lw
      v1.5

这时，如果在标签上运行 git show，你不会看到额外的标签信息。

      $ git show v1.4-lw
      commit ca82a6dff817ec66f44342007202690a93763949
      Author: Scott Chacon <schacon@gee-mail.com>
      Date:   Mon Mar 17 21:52:11 2008 -0700

      changed the version number
#### 删除标签    
删除本地标签

    git tag -d v1.0.3
删除远程标签

    git push origin –delete v1.0.3

***

#### 删除远程分支

    git push origin :branch-name

冒号前面的空格不能少，原理是把一个空分支push到server上，相当于删除该分支。

***

## <a id="git_status">git 使用技巧<a/>

1 在本地仓库执行 pull 时报以下错误：

    sign_and_send_pubkey: signing failed: agent refused operation
执行以下命令：

    eval "$(ssh-agent -s)"
    ssh-add

***

2 当在本地执行 push 推送到远程仓库中时，发现代码有重大问题，想要删除该条提交记录时，可以按以下步骤执行：

    git reset --hard SHA 
这句命令可以回退到指定版本。再次提交时如果保存，加上 -f 参数：

    git push -u master origin -f
此时，SHA 对应的版本之后所有的版本就被覆盖了，所以需要谨慎处理，最好做个备份。

***

3 HEAD 的含义
在git中，用HEAD表示当前版本，也就是最新的提交，比如：3628164...882e1e0，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。

 * HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id。
 * 穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。
 * 要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。

***

 4 如何江本地项目与远程仓库（如：github）关联    
 * 首先将本地项目转换成 git 项目：
 
        git init
        git add .
        git commit -m 'say something...'
 * 接着在远程仓库如 github 创建一个同名项目，创建后获取仓库地址，执行以下命令：
 
        git remote add origin git@github.com:yourname/仓库名.git
 * 现在可以提交代码：
 
        git push -u origin master
   如果初次提交的代码与服务器有冲突，需要先执行 git pull 同步代码。
   
.gitkeep文件

git 是不允许提交一个空的目录到版本库上的,可以在空的文件夹里面建立一个.gitkeep文件，然后提交去即可。其实在git中 .gitkeep 就是一个占位符。
   
***




