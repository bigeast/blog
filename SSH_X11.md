#14/04/13/00:55:24/Sunday
实验室的网络是所有的机器都连到一个路由器上，共用一个外网IP，这样在宿舍就不能ssh到实验室的电脑了。

这个问题困扰了我很久。我知道有些软件是可以在这种情况下远程控制实验室的电脑的，例如teamviewer，但问题是它似乎没有通用的64位linux版本，而且必须连外网，更关键的是得通过它的服务器来转发数据，安全性自己不能控制。

昨天问了宿舍的大神，一句话就基本解决了我的问题，让我深感自己还是菜鸟。也让我知道了我们实验室的网络设置是多么奇葩，因为其它实验室都是，每台机器都有自己的IP，THU好像有6个B类IP地址段，肯定是够用的，没有必要大家共用一个IP，而且通过路由器好像还上不了IPv6，速度最多也就2M，应该就是路由器的限制，一个人登录校园网帐号大家都可以用这一个人的流量，有点狠。

解决方法就是在路由器上设置端口映射，我今天尝试了一下，挺简单的。
实验室路由器是Cisco WRVS4400N，登录路由器设置界面（默认是192.168.1.1），选择 **Firewall** -> **Single Port Forwarding** ，然后加上这样一条规则：

**Application 	|External Port 	|Internal Port 	|Protocol 	|IP Address	|Enabled**
NATssh			8051			22				TCP			192.168.1.129	☑

就可以了。我是先ssh到宿舍的电脑，然后又ssh回来，测试成功了。是不是可以一直这样来回ssh。。

按照[ArchWiki](https://wiki.archlinux.org/index.php/Secure_Shell#X11_forwarding)上的教程可以设置通过ssh转发X11窗口，但是简直太慢了，不可忍受的速度。

找到了[how-to-speed-up-x11-forwarding-in-ssh](http://xmodulo.com/2013/07/how-to-speed-up-x11-forwarding-in-ssh.html)，通过数据压缩，提高速度的方法。效果明显好很多了。

但是还有一种更有针对性的解决方案是[NX technology](http://en.wikipedia.org/wiki/NX_technology)，在Archlinux里有freenx可以用，但是现在官方已经不支持了，取而代之的是[X2Go](https://wiki.archlinux.org/index.php/X2Go)，十分好用，竟然还可以另外选择窗口管理器和桌面环境。

在X2Go里打开virtualbox，然后登录VS调试程序，跑起来比我笔记本上的还快。

另外ssh的密钥管理我还没有搞明白，应该是有被中间人攻击的风险，但是无所谓了。。

还有就是如果实验室IP，或者分给我实验室电脑的局域网IP变的话，应该就会出问题。但最近一个多月都没变了，就这样吧。

想找个时间，把实验室网络搞得正常点，最起码能上IPv6啊。几个师兄好像都不怎么关心这些，也不经常来实验室。。

嗯，这样似乎实验室电脑就成了我的私人服务器了，16G内存，2T硬盘，而且校园网之间不费流量的，可以各种scp，rsync，缓解我笔记本的硬盘压力。

刚买了一个TP-link的路由器应该这周就会到，路由器还真是个好东西。
