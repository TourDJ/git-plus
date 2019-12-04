
## 删除提交的记录

### 删除最后一次提交

        git revert HEAD
        git push origin master

或者

        git reset --hard HEAD^
        git push -f

> revert 和 reset 的区别       
> revert是放弃指定提交的修改，但是会生成一次新的提交，需要填写提交注释，以前的历史记录都在，而reset是指将HEAD指针指到指定提交，历史记录中不会出现放弃的提交记录。git revert是用一次新的commit来回滚之前的commit，git reset是直接删除指定的commit。

### 删除历史某次提交
1. 查看历史纪录，找到 commit_id

        git log --oneline
相关命令：

        # 显示每次修改的文件列表及修改状态
        git log --name-status 
        # 显示每次修改的文件列表
        git log --name-only 
        # 显示每次修改的文件列表及文件修改的统计
        git log --stat 
        # 显示每次修改的文件列表
        git whatchanged 
        # 显示每次修改的文件列表及统计信息
        git whatchanged --stat
        # 显示最后一次的文件改变的具体内容
        git show 

2. 删除提交记录

        git reset --hard <commit_id>

根据`–soft`，`–mixed`, `–hard`，会对working tree 、index和HEAD进行重置:
* git reset –mixed：此为默认方式，不带任何参数的git reset，即时这种方式，它回退到某个版本，只保留源码，回退commit和index信息
* git reset –soft：回退到某个版本，只回退了commit的信息，不会恢复到index file一级。如果还要提交，直接commit即可
* git reset –hard：彻底回退到某个版本，本地的源码也会变为上一个版本的内容

<commit_id> 包括：
* HEAD 最近一个提交     
* HEAD^ 上一次      
* <commit_id> 每次commit的SHA1值. 可以用git log 看到,也可以在页面上commit标签页里找到      
