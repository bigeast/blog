之前没怎么使用过打印机，在Windows下都是自动在局域网中识别出来打印机，只要选择就行了。实验室有一台打印机，连在一个装Windows 7的电脑上，实验室的电脑都是通过路由器连接。为了不用每次打印东西都要换个系统，我一直想研究怎么在Linux下用上那台打印机。

搜到了[CUPS(Common Unix Printing System)](http://en.wikipedia.org/wiki/CUPS)，今天终于配置成功了。

开始以为可以通过局域网IP直接相连，就按照添加网络打印机的教程来做。后来意识到，那个IP是与打印机相连的电脑的，不是打印机自己的IP，应该不行。

所以就参考[Archwiki](https://wiki.archlinux.org/index.php/CUPS_printer_sharing#Sharing_via_Samba_2)中的（最简单的）方法，最后成功连接到了打印机。

打印机型号是**HP LaserJet P2055**，好像很老了。
* * *
### 在Archlinux中添加Windows机器的打印机。
* 安装cups,samba
* systemctl start cupsd
* sudo smbd _# 用systemctl开启的时候有问题_
* 浏览器打开 _http://localhost:631_ _# 界面一看银灰色就感觉像是苹果的_
* 在Administration中，选择添加打印机
* 选择Windows Printer via SAMBA，然后继续
* 连接地址写为 _smb://video@file/HP LaserJet P2050 Series PCL6_ ，**注意要把空格换成%20**
* 填写Name，Description，Location
* Make选择HP
* Model选择HP Color LaserJet Series PCL 6 CUPS(en)
* 添加打印机！
### 命令行中打印！
* lpstat -p _# 显示可用打印机名称_
* lpoptions -d HP _# 将刚才添加的打印机设为默认_
* lp file.pdf _# 成功！_
### 打印课件PPT
* 一般每页打印六页，双面打印。
* 可以用pdfjam先对课件进行处理：_pdfjam --nup 2x3 file.pdf_
* _lp -o sides=two-sided-long-edge file-pdfjam.pdf_ _# 第一次尝试的时候把two-sided写成了two-side，结果还取消不了，于是单面打印了，浪费纸张，罪过_
