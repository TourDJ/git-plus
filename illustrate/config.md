
  - [属性配置](#config)
    - [颜色配置](#config_color)      
 
## <a id="config">属性配置</a> 
Git 使用一系列的配置文件来存储你定义的偏好:

1.它首先会查找`/etc/gitconfig`文件，该文件含有对系统上所有用户及他们所拥有的仓库都生效的配置值， 如果传递`--system`选项给<strong>git config</strong>命令， Git 会读写这个文件。

2.接下来 Git 会查找每个用户的`~/.gitconfig`文件，你能传递`--global`选项让 Git 读写该文件。

3.最后 Git 会查找由用户定义的各个库中 Git 目录下的配置文件（`.git/config`），该文件中的值只对属主库有效。 

以上阐述的三层配置从一般到特殊层层推进，如果定义的值有冲突，以后面层中定义的为准，例如：在`.git/config`和`/etc/gitconfig`的较量中， `.git/config`取得了胜利。虽然你也可以直接手动编辑这些配置文件，但是运行<code>git config</code>命令将会来得简单些。

### 设置名字和邮箱地址

    $ git config --global user.name "John Doe"
    $ git config --global user.email johndoe@example.com

### <a id="config_color">颜色配置</a>
git 命令行的颜色配置

* 默认的颜色设置
```
    启用 git config --global color.ui true
    关闭 git config --global color.ui false
```
* 针对具体的内容进行设置
```
    git config --global color.status auto 
    git config --global color.diff auto 
    git config --global color.branch auto 
    git config --global color.interactive auto
```
例如：

    git config --global color.diff.meta "blue black bold"
这样会将diff的输出以蓝色字体，黑色背景，粗体显示。

◎ 颜色可用值有：

    normal
    black
    red
    green
    yellow
    blue
    magenta
    cyan
    white

◎ 字体可选值有:

    bold
    dim
    ul
    blink
    reverse

