- [进阶命令](#advanced_command)      
  - [查看远程仓库](#git_remote)      
    - [添加远程仓库](#git_remoteadd)      
    - [查看远程仓库详细信息](#git_remoteshow)       
    - [远程仓库的重命名](#git_remoterename)      
    - [远程仓库的移除](#git_remoterm)     
    - [更新获取地址方式](#git_remoteseturl)    
  - [从远程仓库中拉取](#git_fetch)     
  - [从远程仓库中抓取与合并](#git_pull)     
  - [推送到远程仓库](#git_push)     
  - [储藏你的工作](#git_stash)         
  - [逐行显示该文件的修改记录](#git_blame)         
  - [远程仓库回滚](#git_rollback)                
  - [比较文件的差异](#git_diff)  
  - [存储用户名与密码](#git_username) 

## <a id="advanced_command">进阶命令</a>

### <a id="git_remote">查看远程仓库<a/>
`git remote`: 查看远程仓库
  
    $ git remote
    origin
你也可以指定选项 -v，会显示需要读写远程仓库使用的 Git 保存的简写与其对应的 URL。

    $ git remote -v
    origin  https://github.com/schacon/ticgit (fetch)
    origin  https://github.com/schacon/ticgit (push)

#### <a id="git_remoteadd">添加远程仓库<a/>
`git remote add`: 添加远程仓库
在本地的git仓库"添加一个远程仓库",当然这个远程仓库还是你自己的这个目录。
$ git remote add origin ssh://你的IP/~/testgit/.git
    
    $ git remote
    origin
    $ git remote add pb https://github.com/paulboone/ticgit
    $ git remote -v
    origin  https://github.com/schacon/ticgit (fetch)
    origin  https://github.com/schacon/ticgit (push)
    pb  https://github.com/paulboone/ticgit (fetch)
    pb  https://github.com/paulboone/ticgit (push)
添加一个新的远程 Git 仓库，同时指定一个你可以轻松引用的简写。


#### <a id="git_remoteshow">查看远程仓库详细信息<a/>
git remote show 查看远程仓库详细信息

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

#### <a id="git_remoterename">远程仓库的重命名<a/>
git remote rename 远程仓库的重命名

    $ git remote rename pb paul
    $ git remote
    origin
    paul

#### <a id="git_remoterm">远程仓库的移除<a/>
git remote rm 远程仓库的移除

    $ git remote rm paul
    $ git remote
    origin

### <a id="git_remoteseturl">更新获取地址方式<a/>
git remote set-url 更新获取地址方式
> set-url 有两个参数：当前远程库名字、为远程库新设置的地址

    git remote set-url origin http://git.example.cn:3000/project/ptest.git

***

### <a id="git_fetch">从远程仓库中拉取<a/>
`git fetch`: 从远程仓库中拉取
这个命令会访问远程仓库，从中拉取所有你还没有的数据。 执行完成后，你将会拥有那个远程仓库中所有分支的引用，可以随时合并或查看。

    $ git fetch [remote-name]
git fetch origin 会抓取克隆（或上一次抓取）后新推送的所有工作。 必须注意 git fetch 命令会将数据拉取到你的本地仓库 - 它并不会自动合并或修改你当前的工作。 当准备好时你必须手动将其合并入你的工作。
***

### <a id="git_pull">从远程仓库中抓取与合并<a/>
git pull 从远程仓库中抓取与拉取
自动的抓取然后合并远程分支到当前分支。 

    $ git pull [remote-name]
***

### <a id="git_push">推送到远程仓库<a/>
git push 推送到远程仓库
    $ git push origin master
当你和其他人在同一时间克隆，他们先推送到上游然后你再推送到上游，你的推送就会毫无疑问地被拒绝。 你必须先将他们的工作拉取下来并将其合并进你的工作后才能推送。
***

### <a id="git_stash">储藏你的工作<a/>
git stash 储藏你的工作
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

### <a id="git_blame">逐行显示该文件的修改记录<a/> 
git blame 逐行显示该文件的修改记录
命令模式会显示所有行的信息。我们可以利用"<开始>，<结束>"表示，<开始>和<结束>参数可以是数字。

    $ git blame -L 12,13 hello.html 
将显示第12、13行代码的信息
<结束>这个参数不必非得指定一个数字，可以使用+N或者-N指定范围。
    
    $ git blame -L 12,-3 hello.html
也可以使用正则表达式进行匹配行内容，和已经查询以往某个版本中的内容。如：

    $ git blame -L "/<\/body>/",-2 4333289e^ -- index.html
注意上面有--，这是通知Git查询指定文件。

***

### <a id="git_rollback">远程仓库回滚<a/>

#### 删除最后一次提交 git revert


    git revert HEAD
    git push origin master

* revert是放弃指定提交的修改，但是会生成一次新的提交，需要填写提交注释，以前的历史记录都在; 
* 而reset是指将 HEAD 指针指到指定提交，历史记录中不会出现放弃的提交记录。

#### 删除历史某次提交(git rebase) 
这种情况需要先用git log命令在历史记录中查找到想要删除的某次提交的commit id， 然后执行以下命令：

    git rebase -i "commit id"^
"commit id"替换为想要删除的提交的"commit id"，需要注意最后的^号，意思是commit id的前一次提交。
命令:

    git rebase -i  [startpoint]  [endpoint]
其中-i的意思是--interactive，即弹出交互式的界面让用户编辑完成合并操作，[startpoint] 和 [endpoint]则指定了一个编辑区间，如果不指定[endpoint]，则该区间的终点默认是当前分支HEAD所指向的commit(注：该区间指定的是一个前开后闭的区间)。

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

### <a id="git_diff">比较文件的差异<a/>
git diff 比较文件的差异
[git diff](http://blog.csdn.net/csfreebird/article/details/8044796) 可以比较working tree同index之间，index和git directory之间，working tree和git directory之间，git directory中不同commit之间的差异。

***

### <a id="git_username">存储用户名与密码<a/>
保存在内存中：

    git config --global credential.helper cache
which tells git to keep your password cached in memory for (by default) 15 minutes. You can set a longer timeout with:
    
    git config --global credential.helper "cache --timeout=3600"

保存在磁盘上：

    git config --global credential.helper store

***

### create a new repository on the command line
```
echo "# locallibrary" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin git@github.com:TourDJ/locallibrary.git
git push -u origin master
```
当拉取代码是出现： `fatal: refusing to merge unrelated histories`
执行以下代码

    git pull origin master --allow-unrelated-histories
    git push --set-upstream origin master






