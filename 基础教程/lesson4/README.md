# <<创建store>>

Magento最强大的特点之一是可以从一个后台管理多个网站和商店。

这使得店主可以管理不同网址的商店，在一个网址上可以用不同语言显示相同的产品,以及其它各种设置。

如果你只是在一个网址上用一种语言来卖你的产品，那你就不需要此功能，但是可轻易扩展更多语言的能力使得Magento可以随着你的电子商务业务壮大而升级。

网站（website）
```
一个网站可以包含一个或多个商店，而且这些商店是共享相同的顾客信息，订单信息以及购物车信息。这是一个广泛的概念，商家可以根据自己的特殊要求来设定网站。
```

商店（store）
```
商店可以由多种不同方式建立，但是需要提醒的是如果它们是属于同一个网站的话，它们将共享某些信息。
```

商店界面（store views）
```
商店界面主要在使用不同语言时应用，举个例子，如果商店支持英语和西班牙语，那么你只需创建一次商店并为它创建两个不同的商店界面。
```

magento是一个多网站多网店多语言的系统.可谓强悍至极。

###案例分析

比如:
我有2个网站,都是卖婴儿用品的。

一个是B2C的(http://b2c.hongzhao.com).

一个是B2B的(http://b2b.hongzhao.com).

这2个网站都是多语言站,都是面向美国和芬兰的,所以有英语版和芬兰语版。

按照传统思维,我得建4个网站。分别是:
```
b2c:
 英文站: http://b2c.hongzhao.com(默认)
 芬兰语站: http://b2c-fi.hongzhao.com
b2b:
 英文站: http://b2b.hongzhao.com(默认)
 芬兰语站: http://b2b-fi.hongzhao.com
```

现在有了magento后,就不用那么纠结蛋疼了。直接一步就可以在后台设置好,简单粗暴,一个后台,易于管理。


####如何配置

一: 配置stores。 

进入System->Manage Stores

先编辑默认的stores。要依次从Store View ＝> Store =>Website编辑。因为他们是相互关联的。

如图,编辑Store View
![](https://raw.githubusercontent.com/zouhongzhao/magento-lessons/master/基础教程/lesson4/store-1.png)

如图,编辑Store
![](https://raw.githubusercontent.com/zouhongzhao/magento-lessons/master/基础教程/lesson4/store-2.png)

如图,编辑Website
![](https://raw.githubusercontent.com/zouhongzhao/magento-lessons/master/基础教程/lesson4/store-3.png)

接下再创建B2C的。

最后如图:

![](https://raw.githubusercontent.com/zouhongzhao/magento-lessons/master/基础教程/lesson4/store-4.png)

二:设置域名

进入System -> Configuration

![](https://raw.githubusercontent.com/zouhongzhao/magento-lessons/master/基础教程/lesson4/domain-1.png)

先设置B2B英文站的：

![](https://raw.githubusercontent.com/zouhongzhao/magento-lessons/master/基础教程/lesson4/domain-2.png)

设置域名后,保存.

依葫芦画瓢,依次设置其他网店的域名。

三: apache配置域名

```
<VirtualHost *:80>
    ServerName demo.hongzhao.com
	ServerAlias wwww.demo.hongzhao.com b2b.hongzhao.com b2c.hongzhao.com b2b-fi.hongzhao.com b2c-fi.hongzhao.com
    DocumentRoot /Applications/MAMP/htdocs/demo/
    SetEnvIf Host b2b\.hongzhao\.com MAGE_RUN_CODE=en
    SetEnvIf Host b2b\.hongzhao\.com MAGE_RUN_TYPE=store
	SetEnvIf Host b2b-fi\.hongzhao\.com MAGE_RUN_CODE=fi
	SetEnvIf Host b2b-fi\.hongzhao\.com MAGE_RUN_TYPE=store
	SetEnvIf Host b2c\.hongzhao\.com MAGE_RUN_CODE=cen
	SetEnvIf Host b2c\.hongzhao\.com MAGE_RUN_TYPE=store
	SetEnvIf Host b2c-fi\.hongzhao\.com MAGE_RUN_CODE=cfi
    SetEnvIf Host b2c-fi\.hongzhao\.com MAGE_RUN_TYPE=store
	<IfModule mpm_itk_module>
	AssignUserId iggo iggo 
	</IfModule>

        <Directory /Applications/MAMP/htdocs/demo/>
                AllowOverride all
                #AddHandler fcgid-script .php
                AddHandler cgi-script .cgi .pl
                Options +ExecCGI -Indexes
                Allow from all
        </Directory>

        CustomLog /var/log/apache2/vhost_access.log combined
        #CustomLog /home/magento/demo/logs/access.log combined
        #ErrorLog /home/magento/demo/logs/error.log

        <IfModule mod_rewrite.c>
             RewriteEngine on
             RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization},L]
        </IfModule>

</VirtualHost>

```

四:访问域名,看是否能打开

不出意外的话,会很贴心的符合你预期的出来了。



