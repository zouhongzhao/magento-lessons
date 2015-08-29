# <<关于插件安装后没反应的解决方法>>

以插件Faarao_Colorpick为例。

1,先去后台system→configuration→advanced

查看有没有Faarao_Colorpick模块,是否enable。如果有的话直接看第3步，没有的话看第2步

2,如果没有的话，就看app/etc/modules/Faarao_Colorpick.xml。

仔细检查有没有写错，一般是按照下面的写法:
```
<?xml version="1.0" encoding="utf-8" ?>
<config>
    <modules>
        <Faarao_Colorpick>
            <active>true</active>
            <codePool>local</codePool>
        </Faarao_Colorpick>
    </modules>
</config>
```

3,如果有的话,就检查config.xml中的<resources></resources>标签内容是否在<global>内。一定要确保在<global>标签里面,别写到外面去了

4,还不行的话,就检查插件文件的权限问题，都chmod 777 * -R.再试

5,仍然不行的话,估计是改错文件或者搞错网店了。再不行的话就发生灵异现象了

参考:

load config.xml之类的代码参考

Mage_Core_Model_Config 中的_loadDeclaredModules和loadModulesConfiguration

Varien_Simplexml_Config