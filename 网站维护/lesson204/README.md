# <<打印invoice的pdf时报错>>

在后台打印invoice的pdf时,线上服务器报错: 

```
Fatal error: Declaration of Zend_Pdf_FileParserDataSource_File::construct() must be compatible with Zend_Pdf_FileParserDataSource::construct() in /var/www/topsport/lib/Zend/Pdf/Zend_Pdf_FileParserDataSource_File.php on line 41
```

解决办法:

```
This an incompatibility issue between PHP Version 5.4.4 and zend Framwork .

Fixed it by change in this function lib/Zend/Pdf/FileParserDataSource.php.

change

abstract public function __construct();

to

abstract public function __construct($filePath);
```

这一般出现在1.7.2的版本。