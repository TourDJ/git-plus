# git-plus
Here is git playground to deep explore git's operation and run mechanism, record git's daily common command, mealwhile also descrilbe some classic scenario. 

Enjoy yourself!

## What's git?
摘录一段[官方](https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F)介绍：    
Gist is a VCS, which is different from others, such as cvs, svn, git thinks of its data more like a series of snapshots of a miniature filesystem. With Git, every time you commit, or save the state of your project, Git basically takes a picture of what all your files look like at that moment and stores a reference to that snapshot. To be efficient, if files have not changed, Git doesn’t store the file again, just a link to the previous identical file it has already stored. Git thinks about its data more like a stream of snapshots.       

## How git works?
First we see the git theory graphic:
![Git theory](https://github.com/TourDJ/git-plus/blob/master/images/git-theory.jpg)     

Then base on the graphic explain how git works:
Apparently, git have local and remote repository, index and workspace area:

* workspace: our local work playground, when we change the code and exec `git add *` will commit the change to workplace.
* index: staged area.
* repository: local repostory.
* remote: remote repository, such as github, gitlab, i.e.


Remember that each file in your working directory can be in one of two states: tracked or untracked. 

![lifecycle](https://github.com/TourDJ/git-plus/blob/master/images/lifecycle.png)   

