# <<如何在layout中对addJs/addCss加载的文件排序>>

在magento模板开发中,首页的幻灯片一般用owl插件,都用的Jquery。

经常会出现
```
TypeError: jQuery(...).owlCarousel is not a function
```
的错误。

是什么原因呢？

其实就是js冲突了。owl插件的js被放到jQuery前面去了。

可是我layout里明明是把jQuery.js放在第一的啊,为啥在前台页面会跑到下面来呢？？？

因为当前主题其他地方可能已经引入了jQuery,你再加载一次的话会重复,最好的办法是注释掉你当前layout里的jQuery.js，然后把owl的js都放到最后面去加载。但是addJs没有这个排序的设置.

这里有个取巧的方法,就是利用params,进行分组。

比如这样：
```
<reference name="head">
<action method="addJs">
    <script>owl.carousel.0.js</script>
    <params><![CDATA[data-group="js002"]]></params>
    <!-- creates first group js002 -->
</action>
<action method="addJs">
    <script>owl.carousel.1.js</script>
    <params><![CDATA[data-group="js002"]]></params>
    <!-- appends to created group js002, after javascript0 -->
</action>
<action method="addJs">
    <script>owl.carousel.2.js</script>
    <params><![CDATA[data-group="js001"]]></params>
    <!-- creates first group js001 -->
</action>
<action method="addJs">
    <script>owl.carousel.3.js</script>
    <params><![CDATA[data-group="js001"]]></params>
    <!-- appends to created group js001, after javascript2 -->
</action>
<action method="addJs">
    <script>owl.carousel.4.js</script>
    <params><![CDATA[data-group="js002"]]></params>
    <!-- appends to created group js002, after javascript1 -->
</action>
<action method="addJs">
    <script>jQuery.js</script>
    <!-- no params supplied, will be rendered before any groups -->
</action>
</reference>
```
它的输出顺序是:
```
jQuery.js (no params, comes first)
owl.carousel.0.js (first created params group "js002")
owl.carousel.1.js (first created params group "js002")
owl.carousel.4.js (first created params group "js002")
owl.carousel.2.js (second created params group "js001")
owl.carousel.3.js (second created params group "js001")
```

css的也同理

所以这样的话,就不会出现加载顺序出错的问题了。

###备注：

我之前也想到能否在foot或者footer里加,比如这样：
```
<reference name="foot">
<action method="addJs">
    <script>owl.carousel.js</script>
</action>
</reference>
```

但是很可惜,magento默认不支持,foot里面没有getCssJsHtml的代码,除非自己写插件改写原生的block才行。

也许这是magento设计上的某种缺陷吧。理论上js文件都应该在底部footer里加载的。
