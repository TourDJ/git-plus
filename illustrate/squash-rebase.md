
## git merge --squash 和 rebase 区别

git 在使用 `merge pull request` 合并代码的时候，有三种方式：

* git merge
* git merge --squash
* git rebase --interactive

三种方式都可以完成代码的合并，但实现的过程和结果是不一样的。

### git merge abranch

将指定提交的更改合并到当前分支中。 git pull使用此命令来合并来自另一个存储库的更改，并且可以手动使用此命令将更改从一个分支合并到另一个分支。

假定存在以下历史提交记录，当前分支时 "master":

                     A---B---C topic
                    /
               D---E---F---G master

执行 `git merge topic` 将把在 topic分支上从master（即E）分支分出后直到当前提交（即C）所做的更改记录在新提交中，同时记录两个父提交的名称和日志消息。

                     A---B---C topic
                    /         \
               D---E---F---G---H master



### git merge --squash abranc
将生成一个 squash 提交到目标分支中，且不记录其合并的关系，需要主动提交。也即是说，这种合并方式，只拷贝分支中提交的内容到目标分支，但不会保存提交记录，需要额外使用 `git commit -m "squash branch"` 命令增加一个提交。

这在如果你要完全丢弃源分支时的情况下非常有用。

例如，对于以下分支：

    git checkout stable

          X                   stable
         /                   
    a---b---c---d---e---f---g develop

执行：

    git merge --squash develop      
    git commit -m "squash develop"
将变成这样：

          X-------------------G stable
         /                   
    a---b---c---d---e---f---g develop
最后，删除 develop 分支。

### git rebase --interactive
记录下该分支中部分或全部提交内容，添加到目标分支中
replays some or all of your commits on a new base, allowing you to squash (or more recently "fix up", see this SO question), 

还以上面的例子说明，执行：

    git checkout develop
    git rebase -i stable

执行后变成这样：

          stable
          X-------------------G tmp
         /                     
    a---b


所以，[squash 和 rebase 的不同点](https://stackoverflow.com/questions/2427238/in-git-what-is-the-difference-between-merge-squash-and-rebase)是：
* squash 不会改变原分支内容，而是创建一个单独的提交内容
* rebase 是在原分支上操作，要么创建一个新基，要么保持一个干净的历史。
