
## git 更新 fork 的 repository

1. 检查一下当前配置，查看是否设置了 upstream

	$ git remote -v
	origin  https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
	origin  https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)

2. 将原 repository 设置为自己 fork 出的 repository 的 upstream

	$ git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git

3. 再次查一下当前配置：

	$ git remote -v
	origin    https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
	origin    https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)
	upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (fetch)
	upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (push)
发现多了 upstream 配置	

4. fetch 源分支的新版本到本地

	$ git fetch upstream


5. 将 upstream/master 上的更新合并到本地的 master 上

	$ git merge upstream/master

> 如果没有冲突，将执行 "Fast-forward" 合并，如果有冲突，则要解决冲突后再合并，如果使用upstream/master上的来覆盖本地则无需解决冲突了，执行：
	
	$ git merge -X theirs upstream/master

