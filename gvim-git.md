title: gvim与git的交互
date: 2014-05-03 09:18:32
tags:
---
如果git的默认编辑器设置成了gvim，那么每次git commit，输入日志wq后，回到终端会提示：**"Aborting commit due to empty commit message."** ，然后输入的内容也丢失掉了。

最近终于找到了问题，参考的stackoverflow上的[一个帖子](http://stackoverflow.com/questions/3764481/git-commit-fails)。

就是说，编辑器是从终端开的，终端的程序最终要与编辑器保持联系直到保存退出。然而为了方便，gvim从终端开启后默认detach，脱离终端，独立出去，这样可以在终端继续输入其它命令而不用管gvim如何。但是这样终端就完全不知道gvim里编辑了什么，何时退出，它根本无法收到编辑的内容！

为了阻止gvim自动detach出去，需要加上-f参数：

<font color='green'>
	$git config --global core.editor "gvim -f"
</font>

Problem Solved.

然后今天碰巧用了下 **crontab -e** ，试了好几次都不能正确安装计划任务。后来想到了可能是相同的问题，crontab是根据环境变量 ** EDITOR **来选择编辑器，同样给gvim加上-f参数就好了：

<font color='green'>
	export EDITOR="gvim -f"
</font>

但是不知道其它同样依赖**EDITOR**的程序会不会出问题，可能还是用"vim"最安全了吧。

然后，今天还了解了一下如何创建daemon进程，简单说就是两次fork：

- 在进程A中fork出B，让A退出
- 在B中fork出C，并让B退出，这样C就成了daemon进程。

如果fork B后，A退出，B是不是daemon进程呢？为什么必须要fork两次呢？去图书馆看了很薄的《理解Unix进程》后，大概明白了：进程A也有父进程，例如写了一个创建daemon的C程序，然后在bash中运行，这个C程序就是进程A，它虽然在fork B之后退出了，但是B还是在A原先所在的那个 **进程组** 这样B还是没能与bash完全脱离，还能被bash进程发出的信号影响到。再fork C的话，C与B在同一个进程组，B是这个组的领导，B退出之后，C就独立了。

大概是这样。书中还显式地把B设为了组的领导，不知道是否必要。
