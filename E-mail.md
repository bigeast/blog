> 14/12/27/21:08:59/星期六
msmtp 
=====
* 查询SMTP服务器信息:
	msmtp --host=smtp.qq.com --serverinfo
* ~/.msmtprc 
	* 权限必须是 *600*
	* 可以不写明文密码，用 *passwordeval*，需要先把密码用gpg加密，然后每次需要
	  	密码的时候用gpg解密。开gpg-agent比较方便。
	* [msmtprc.txt](http://msmtp.sourceforge.net/doc/msmtprc.txt)

DKIM 
=====
	dkim.org
	DomainKeys Identified Mail (DKIM) 
	大概是服务器转发邮件的凭证，建立一种信誉系统。
