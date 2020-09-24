

## 常见问题

§ `How is a tag different from a branch in Git? `      

* From the ***theoretical*** point of view:

tags are symbolic names for a given revision. They always point to the same object (usually: to the same revision); they do not change.

branches are symbolic names for line of development. New commits are created on top of branch. The branch pointer naturally advances, pointing to newer and newer commits.

* From the ***technical*** point of view:

tags reside in refs/tags/ namespace, and can point to tag objects (annotated and optionally GPG signed tags) or directly to commit object (less used lightweight tag for local names), or in very rare cases even to tree object or blob object (e.g. GPG signature).

branches reside in refs/heads/ namespace, and can point only to commit objects. The HEAD pointer must refer to a branch (symbolic reference) or directly to a commit (detached HEAD or unnamed branch).
remote-tracking branches reside in refs/remotes/<remote>/ namespace, and follow ordinary branches in remote repository <remote>.

More detail see [How is a tag different from a branch in Git?](https://stackoverflow.com/questions/1457103/how-is-a-tag-different-from-a-branch-in-git-which-should-i-use-here)  



§ `fatal: refusing to merge unrelated histories`

如果合并了两个不同的开始提交的仓库，在新的 git 会发现这两个仓库可能不是同一个，为了防止开发者上传错误，于是就给下面的提示

    fatal: refusing to merge unrelated histories
git 不让提交，认为是写错了 origin ，如果确定是这个 origin 就可以使用 --allow-unrelated-histories 告诉 git 自己确定

    git pull [origin master] --allow-unrelated-histories
如果有设置了默认上传分支就可以用下面代码

    git pull --allow-unrelated-histories



§ `fatal: Unable to create 'E:/workspace2/ProductCertificate/.git/index.lock': File exists.`

Another git process seems to be running in this repository, e.g. an editor opened by 'git commit'. 
Please make sure all processes are terminated then try again. If it still fails, a git process may have crashed in this repository earlier: 
remove the file manually to continue.

.git下的index.lock文件，在进行某些比较费时的git操作时自动生成，操作结束后自动删除，相当于一个锁定文件，目的在于防止对一个目录同时进行多个操作。有时强制关闭进行中的git操作，这个文件没有被自动删除，之后就无法进行其他git操作，必须手动删除。

解决方法:                
找到.git/index.lock文件，直接删除即可，或者执行git命令

    git clean -f .git/index.lock

