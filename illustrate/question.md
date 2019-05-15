

#### fatal: refusing to merge unrelated histories
如果合并了两个不同的开始提交的仓库，在新的 git 会发现这两个仓库可能不是同一个，为了防止开发者上传错误，于是就给下面的提示

    fatal: refusing to merge unrelated histories
git 不让提交，认为是写错了 origin ，如果确定是这个 origin 就可以使用 --allow-unrelated-histories 告诉 git 自己确定

    git pull [origin master] --allow-unrelated-histories
如果有设置了默认上传分支就可以用下面代码

    git pull --allow-unrelated-histories
