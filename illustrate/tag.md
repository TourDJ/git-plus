
  - [标签](#tag)     
    - [创建标签](#tag_create)       
    - [删除标签](#tag_delete)      
    - [查看标签](#tag_query)      
    

## <a id="tag">标签</a>
Git 可以对某个版本打上标签(tag)，表示本版本为发行版。

### <a id="tag_create">创建标签</a>   
Git 使用两种主要类型的标签：轻量标签（lightweight）与附注标签（annotated）。     

* 附注标签     
在 Git 中创建一个附注标签是很简单的。 最简单的方式是当你在运行 tag 命令时指定 -a 选项：

      $ git tag -a v1.4 -m 'my version 1.4'
      $ git tag
      v0.1
      v1.3
      v1.4

向特定的提交添加标签

    git tag -a <版本号> <SHA值> -m "<备注信息>"



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
      
### <a id="tag_delete">删除标签</a>    
删除本地标签

    git tag -d v1.0.3
删除远程标签

    git push origin –delete v1.0.3


### <a id="tag_query">查看标签</a>
* 显示所有标签
```
    git tag
```
显示的标签列表，是按字母排序。

* 显示指定版本的标签
```
    git tag -l <版本号>
```

### 推送标签

推送所有标签

    git push origin --tags

推送指定版本的标签

    git push origin <版本号>





