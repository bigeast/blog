title: 'SUID 和 SGID'
date: 2014-05-11 23:55:57
tags: [linux,admin]
---
Linux中的文件除了rwx权限外，还有另外一类权限，**SUID**(**S**et **U**ser **ID** upon execution)和**SGID**(**S**et **G**roup **ID** upon execution)。

* 列出 _/bin/_ 目录下的SUID文件:
``
$find /bin/ -perm -u=s
``
	/bin/passwd
	/bin/gpasswd
	/bin/mount.cifs
	/bin/mutter-launch
	/bin/suexec
	/bin/chsh
	/bin/pkexec
	/bin/chfn
	/bin/newgrp
	/bin/expiry
	/bin/Xorg
	/bin/su
	/bin/sudo
	/bin/rsh
	/bin/fusermount
	/bin/unix_chkpwd
	/bin/ksu
	/bin/rcp
	/bin/mount
	/bin/sg
	/bin/umount
	/bin/rlogin
	/bin/mtr
	/bin/chage

* 列出 _/bin/_ 目录下的SGID文件:
``
find /bin/ -perm -g=s
``
	/bin/mount.cifs
	/bin/x2goprint
	/bin/unix_chkpwd
	/bin/wall
	/bin/write

SUID文件的用处是，任何用户可以文件所有者的权限执行文件。

例如/bin/passwd是属于root用户的，但是普通用户执行该命令也可以修改密码（也就是修改/etc/passwd和/etc/shadow等通常情况下普通用户不能修改的文件），这就是用到了root的权限。
还有ping命令，因为它要通过网卡发送和接收数据包，读写网卡是需要root权限的。将ping设成SUID，那么普通用户不用sudo也可以读写网卡了。

另外，权限的数字表示一般是三位八进制数，例如644，755等。但考虑到SUID位的完整形式应该是四位八进制数：

	-rwxr-sr-x	|	2755
	-rwsr-xr-x	|	4755
	-rwsr-sr-x	|	6755

即保留原先的三位八进制形式，在此基础上，如果有SUID或者SGID位，再加上第零位：SUID对应的值是4,SGID对应的值是2。
