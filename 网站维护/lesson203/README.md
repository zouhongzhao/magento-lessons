# <<后台登录不进去,也不报错>>

在magento搬家的时候,经常会遇到输入正确的用户名和密码,后台登陆不进去的情况。

这个问题常见于1.7版本,主要是session的问题。解决办法如下:

###1,

Go to 项目/app/code/core/Mage/Core/Model/Session/Abstract

Open the file Varien.php

Go to line no. 108

注释掉
```
call_user_func_array(‘session_set_cookie_params’, $cookieParams);
```
![](https://raw.githubusercontent.com/zouhongzhao/magento-lessons/master/基础教程/lesson203/1.png)

###2,

也是这个文件,但是是改另外一处地方

```
把
$cookieParams = array(
'lifetime' => $cookie->getLifetime(),
'path' => $cookie->getPath(),
'domain' => $cookie->getConfigDomain(),
'secure' => $cookie->isSecure(),
'httponly' => $cookie->getHttponly()
);
改成
$cookieParams = array(
'lifetime' => $cookie->getLifetime(),
'path' => $cookie->getPath(),
//'domain' => $cookie->getConfigDomain(),
//'secure' => $cookie->isSecure(),
//'httponly' => $cookie->getHttponly()
);
 
也就是注释掉
'domain' => $cookie->getConfigDomain(),
'secure' => $cookie->isSecure(),
'httponly' => $cookie->getHttponly()
```

