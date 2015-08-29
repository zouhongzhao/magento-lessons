# <<开启维护模式>>

当我们需要对Magento网站做维护进行修改时，我们可能希望顾客暂时不能访问到网站，这时我们需要把Magento设置为维护模式，我们可以在index.php中看到这样一些代码:
```
$maintenanceFile = 'maintenance.flag';
if (file_exists($maintenanceFile)) {
    include_once dirname(__FILE__) . '/errors/503.php';
    exit;
}
```

程序会检测网站根目录是否有maintenance.flag这个文件，如果有的话，将包含/error/503.php这个文件，并退出程序。

因此如果我们需要设置网站为维护模式，只需要在根目录下新建一个空文件，并命名为maintenance.flag即可，用户将会看到这样的页面：

```
Service Temporarily Unavailable
The server is temporarily unable to service your request due to maintenance downtime or capacity problems. Please try again later.
```

但是我们会发现，如果这样设置的话，即使是网站的管理员也只能看到503页面，而不能看到网站的前台，因此我们可以对代码做一些修改，让指定IP的用户可以看到网站的前台：

```
$maintenanceFile = 'maintenance.flag';
$allowedIp = array('127.0.0.1'); // 指定可以访问网站的IP
if (file_exists($maintenanceFile) && ! in_array($_SERVER['REMOTE_ADDR'], $allowedIp)) {
    include_once dirname(__FILE__) . '/errors/503.php';
    exit;
}
```

这样只有管理员可以看到网站的前台了，当网站维护完毕，只需删除maintenance.flag文件即可。