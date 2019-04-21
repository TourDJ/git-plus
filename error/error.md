
## git 

1 在本地仓库执行 pull 时报以下错误：

    sign_and_send_pubkey: signing failed: agent refused operation
执行以下命令：

    eval "$(ssh-agent -s)"
    ssh-add

***

2 当在本地执行 push 推送到远程仓库中时，发现代码有重大问题，想要删除该条提交记录时，可以按以下步骤执行：

    git reset --hard SHA 
这句命令可以回退到指定版本。再次提交时如果保存，加上 -f 参数：

    git push -u master origin -f
此时，SHA 对应的版本之后所有的版本就被覆盖了，所以需要谨慎处理，最好做个备份。
