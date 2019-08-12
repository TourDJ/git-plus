
  - [HEAD 的含义](#head)      
  - [git reset 使用](#reset)     
  

## <a id="head">HEAD 的含义</a>
在 git 中，用HEAD表示当前版本，也就是最新的提交，比如：3628164...882e1e0，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。

 * HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令 `git reset --hard commit_id`。
 * 穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。
 * 要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。

## <a id="reset">git reset 使用</a>
命令格式：

    git reset [ –-soft | –-mixed | –-hard] <commit>
git reset <commit> 的意思就是 把HEAD移到 `<commit>`。
  
其中：      
* --soft：   修改版本库，保留暂存区，保留工作区。
* --mixed：  修改版本库，修改暂存区，保留工作区。（默认参数）
* --hard：   修改版本库，修改暂存区，修改工作区。



