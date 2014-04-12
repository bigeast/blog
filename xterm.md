*xterm*是X windows的标准终端模拟器。

默认的xterm界面是很丑陋的，需要对字体和颜色进行配置。配置的方法有两种：_命令行参数_ 和 _配置文件_

- 基本配置
	- 命令行参数
><font color='green'>$xterm -fg green -bg black -fa Consolas -fw 'STZhongsong'</font>

	- 配置文件

**~/.Xresource**

----------
<font color='green'>

    ! Comments begin with a "!" character.
    !XTerm*background:       rgb:0/30/20
    XTerm*background:       black
    !XTerm*foreground:       rgb:0/99/0
    XTerm*foreground:       gray
    XTerm*cursorColor:      green
    XTerm.vt100.geometry:   80x24
    XTerm*scrollBar:        false
    XTerm*scrollTtyOutput:  false
    ! font
    !XTerm*font:*-fixed-*-*-*-15-*
    !XTerm*faceName: DejaVu Sans Mono
    !XTerm*faceNameDoublesize: STZhongsong
    XTerm*faceName: Consolas
    XTerm*faceNameDoublesize: WenQuanYi Micro Hei Mono
    XTerm*faceSize: 12
    
    ! Alt key as Meta
    XTerm*metaSendsEscape: true
    
    ! Scroll
    ! default is 1024 lines
    XTerm*saveLines: 4096
    XTerm*jumpScroll: true
    XTerm*MiltiScroll: true
    XTerm*fastScroll: true
    
    xclock*update: 1
    xclock*analog: true
    xclock*Foreground: white
    xclock*Background: rgb:0/20/20
</font>

----------

- 一些默认快捷键
	- **复制** 先按左键，再按右键，则选中两次点击之间的内容。
	- **粘贴** 鼠标中键

- 注意
	1. _fc-list_查看可用的字体。
	2. 带有一堆 "-*-*-*-" 的字体名称是X logical font description [ XLFD ](http://en.wikipedia.org/wiki/X_logical_font_description)
	3. 每次修改完~/.Xresource文件后，要运行
	<font color='green'>
		$xrdb -merge ~/.Xresource
	</font>
		修改才会生效
	4. 查看缺省xterm配置的效果：
	<font color='green'>
		$xrdb -all /dev/null
	</font>

- BUG
	1. 查看Scroll line的buffer时，按上下箭头，或者键入新的命令，不会像其它terminal一样自动回到当前命令行，而是必须手动滚到当前命令行。
