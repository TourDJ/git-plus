

## Question

§ `How is a tag different from a branch in Git? `      

* From the ***theoretical*** point of view:

tags are symbolic names for a given revision. They always point to the same object (usually: to the same revision); they do not change.

branches are symbolic names for line of development. New commits are created on top of branch. The branch pointer naturally advances, pointing to newer and newer commits.

* From the ***technical*** point of view:

tags reside in refs/tags/ namespace, and can point to tag objects (annotated and optionally GPG signed tags) or directly to commit object (less used lightweight tag for local names), or in very rare cases even to tree object or blob object (e.g. GPG signature).

branches reside in refs/heads/ namespace, and can point only to commit objects. The HEAD pointer must refer to a branch (symbolic reference) or directly to a commit (detached HEAD or unnamed branch).
remote-tracking branches reside in refs/remotes/<remote>/ namespace, and follow ordinary branches in remote repository <remote>.

See [detail](https://stackoverflow.com/questions/1457103/how-is-a-tag-different-from-a-branch-in-git-which-should-i-use-here)      

§ `fatal: refusing to merge unrelated histories`

如果合并了两个不同的开始提交的仓库，在新的 git 会发现这两个仓库可能不是同一个，为了防止开发者上传错误，于是就给下面的提示

    fatal: refusing to merge unrelated histories
git 不让提交，认为是写错了 origin ，如果确定是这个 origin 就可以使用 --allow-unrelated-histories 告诉 git 自己确定

    git pull [origin master] --allow-unrelated-histories
如果有设置了默认上传分支就可以用下面代码

    git pull --allow-unrelated-histories
