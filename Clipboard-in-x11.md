title: 'Clipboard in X11'
date: 2014-06-19 14:22:37
tags: [X11,tmux,linux]
---

最近突然感觉剪切版好乱，每次都不能正确工作。而且在tmux里我开了vi-copy模式，鼠
标明明选中后，中键也不能粘贴，**C-v** 也不能粘贴。搞得很烦，终于决定了解下这到底
是怎么回事了。

找到了 [这篇博客](http://standards.freedesktop.org/clipboards-spec/clipboards-latest.txt) ，大概是说，**X的剪切板确实挺乱的**。

一共有三个：PRIMARY,SECONDARY和CLIPBOARD。

- 其中CLIPBOARD是Windows中相应的剪切板，就是可以直接用C-v将其内容粘贴到其它程序
中去的，而且只有手动复制操作才会更新。

- 而PRIMARY总是你鼠标选中的内容，不管你是否要复制它。用鼠标中键粘贴到其它程序中
去。

- SECONDARY一般用不到。

用PRIMARY不是很方便，因为它是自动复制的，有时候你一不小心用鼠标选中了一行，就
把你想要复制的东西给覆盖了（对我来说经常发生）。

回到tmux中，我发现默认的快捷键就挺好用的了，**C-b [** 进入复制模式，然后
**Space**开始复制，**Enter** 复制结束，**C-b ]**粘贴。然而现在复制的内容只能在
tmux内部粘贴，其它程序是不能访问它的剪切板的。要查看tmux剪切板的内容，可以
用

```shell
$ tmux show-buffer
```

现在的任务是要把 tmux buffer 里的内容复制到系统剪切板CLIPBOARD中（忘掉PRIMARY吧
）。需要安装 **xclip**，它的用法是

```shell
tmux show-buffer | xclip -selection clipboard
```
可以把这条命令绑定到tmux的快捷键上。这也是我google到的好多人的用法。
例如[这里](http://joncairns.com/2013/06/copying-between-tmux-buffers-and-the-system-clipboard/)的

```shell
bind y run-shell "tmux show-buffer | xclip -sel clip -i" \; display-message "Copied tmux buffer to system clipboard"
```

我这样用后总是有问题，tmux在每次我复制到CLIPBOARD后都会失去响应，就是不能识别
其它命令了。

最后发现Archlwiki上其实是有提到这点的：[ICCCM Selection Integration](https://wiki.archlinux.org/index.php/tmux#ICCCM_Selection_Integration)

原因是xclip从管道得到要复制的内容后，不会关掉STDOUT，而是继续等输入。可以在
tmux的run-shell后加上-b。我觉得好messy。

**xsel** 也可以实现相同的通能。所以我最后是用的 **xsel**。
