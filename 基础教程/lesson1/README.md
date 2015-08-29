# <<搭建magento项目>>

安装过程特别简单。因为我的系统是MAC,搭建环境的工具是MAMP,所以本地配置清单如下:

```
域名: demo.hongzhao.com
项目根目录: /Applications/MAMP/htdocs/demo/
```
###1,配置域名

windows: 一般用WAMP

linux: 一般手动配置lamp或者lnmp环境

mac: 一般用MAMP(MAMP分免费版和收费版Pro,免费版的得手动配置,收费版的可以自动配置,收费版的要50欧元 终身制)。

具体安装过程就不详细表了,大家可以度娘或者狗哥下.

####如果你域名,项目目录,php环境都配置好了的话,就看下面的.否则后果自负.

###2,从官网下载最新版1.9.2 (https://www.magentocommerce.com/download)

###3,把压缩包解压,然后把magento下面的文件都放到项目根目录下

```
如果在linux或者mac下,可以使用命令:
unzip magento-1.9.2.1-2015-08-03-06-33-36.zip
mv magento/* /Applications/MAMP/htdocs/demo/
```
###4,phpmyadmin里新建数据库(比如demo_magento).
可以对这个数据库增加一个权限用户(比如用户名magento,密码123456)。
mac里用的是Sequel Pro

###5,浏览器访问 http://demo.hongzhao.com
按照提示来安装

![](https://raw.githubusercontent.com/zouhongzhao/magento-lessons/master/基础教程/lesson1/install-1.png)

![](https://raw.githubusercontent.com/zouhongzhao/magento-lessons/master/基础教程/lesson1/install-2.png)

耐心的等待几分钟...

![](https://raw.githubusercontent.com/zouhongzhao/magento-lessons/master/基础教程/lesson1/install-3.png)

安装完成后,会提示输入个人信息和后台用户名密码

![](https://raw.githubusercontent.com/zouhongzhao/magento-lessons/master/基础教程/lesson1/install-4.png)

提交后,就可以进入前台(http://demo.hongzhao.com/)和后台(http://demo.hongzhao.com/index.php/admin)了。

![](https://raw.githubusercontent.com/zouhongzhao/magento-lessons/master/基础教程/lesson1/install-5.png)

![](https://raw.githubusercontent.com/zouhongzhao/magento-lessons/master/基础教程/lesson1/install-6.png)

默认前台页面:

![](https://raw.githubusercontent.com/zouhongzhao/magento-lessons/master/基础教程/lesson1/install-7.png)

到此,一个伟大的购物车系统就搭建完毕了。
