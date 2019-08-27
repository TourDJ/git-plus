# git-plus
Here is git playground to deep explore git's operation and run mechanism, record git's daily common command, mealwhile also descrilbe some classic scenario. 

Enjoy yourself!

## Git 是什么?
关于什么是 Git，摘录一段[官方](https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F)说明来介绍一下最好不过了：   
**『**     Gist is a VCS, which is different from others, such as cvs, svn, git thinks of its data more like a series of snapshots of a miniature filesystem. With Git, every time you commit, or save the state of your project, Git basically takes a picture of what all your files look like at that moment and stores a reference to that snapshot. To be efficient, if files have not changed, Git doesn’t store the file again, just a link to the previous identical file it has already stored. Git thinks about its data more like a stream of snapshots.       **』**

大致意思是 Git 的每个版本都是一个完整的系统，也即是包括每次提交是当前包含的所以代码。若你理解了 Git 的思想和基本工作原理，用起来就会知其所以然，游刃有余。

## 如何工作?
Git 是如何工作的呢？

Git 有三种状态，你的文件可能处于其中之一：已提交（committed）、已修改（modified）和已暂存（staged）。 已提交表示数据已经安全的保存在本地数据库中。 已修改表示修改了文件，但还没保存到数据库中。 已暂存表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中。

由此引入 Git 项目的三个工作区域的概念：Git 仓库、工作目录以及暂存区域。

![Git theory](https://github.com/TourDJ/git-plus/blob/master/images/git-theory.jpg)    
Git 的工作原理图

从这张图中可以看出，Git 包含了几个存放文件的库：
* workspace: our local work playground, when we change the code and exec `git add *` will commit the change to workplace.
* index: staged area.
* repository: local repostory.
* remote: remote repository, such as github, gitlab, i.e.

Git 仓库目录是 Git 用来保存项目的元数据和对象数据库的地方。 这是 Git 中最重要的部分，从其它计算机克隆仓库时，拷贝的就是这里的数据。

工作目录是对项目的某个版本独立提取出来的内容。 这些从 Git 仓库的压缩数据库中提取出来的文件，放在磁盘上供你使用或修改。

暂存区域是一个文件，保存了下次将提交的文件列表信
