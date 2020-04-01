
## 克隆最近一个分支
当项目过大时，git clone会出现超时失败，解决方法很简单，在git clone时加上--depth=1即可解决。

    git clone --depth=1 xxxxxx
depth用于指定克隆深度，为1即表示只克隆最近一次commit。

这种方法克隆的项目只包含最近的一次commit的一个分支，体积很小，但会产生另外一个问题，他只会把默认分支clone下来，其他远程分支并不在本地，所以这种情况下，需要用如下方法拉取其他分支：

    git clone --depth 1 xxxxxx
    git remote set-branches origin 'remote_branch_name'
    git fetch --depth 1 origin remote_branch_name
    git checkout remote_branch_name
