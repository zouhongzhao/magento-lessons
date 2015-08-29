# <<创建单独可运行脚本>>

使用以下代码可以创建一个位于magento根目录下的直接可以命令行里运行的文件,

该文件内可以调用任意的magento函数,这样的脚本可以用于测试,调试,或者修正一些bug

```
<?php

ini_set('memory_limit', '-1');

require 'app/Mage.php';

umask (0);

Mage ::app('admin');

$products = Mage::getModel( “catalog/product” )→load(13526);

var_dump ($products→ debug());

```