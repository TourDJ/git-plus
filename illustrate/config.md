
  - [颜色配置](#config_color)      
  

### <a id="config_color">颜色配置</a>
git 命令行的颜色配置

* 默认的颜色设置

    启用 git config --global color.ui true
    关闭 git config --global color.ui false

* 针对具体的内容进行设置

    git config --global color.status auto 
    git config --global color.diff auto 
    git config --global color.branch auto 
    git config --global color.interactive auto

例如：

    git config --global color.diff.meta "blue black bold"
这样会将diff的输出以蓝色字体，黑色背景，粗体显示。

颜色可用值有：

    normal
    black
    red
    green
    yellow
    blue
    magenta
    cyan
    white

字体可选值有:

    bold
    dim
    ul
    blink
    reverse

