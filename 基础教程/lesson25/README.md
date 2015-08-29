# <<magento默认的成功/错误展示信息>>

为了方便提示用户,显示成功或者错误的提示信息的话,可以直接用magento自带的提示信息

错误的信息为:
```
<div id="messages_product_view">
<ul class="messages">
<li class="error-msg">
<ul>
<li>
自定义的显示内容
</li>
</ul>
</li>
</ul>
</div>
```
用jquery可以直接这么写:
```
jQuery("#messages_product_view").html('<ul class="messages"><li class="error-msg"><ul><li><?php echo $this->__("自定义内容") ?></li></ul></li></ul>');
```

成功的信息为:
```
<div id="messages_product_view">
<ul class="messages">
<li class="success-msg">
<ul>
<li>
<span>自定义的显示内容</span>
</li>
</ul>
</li>
</ul>
</div>
```

用jquery可以直接这么写:
```
jQuery("#messages_product_view").html('<ul class="messages"><li class="success-msg"><ul><li><?php echo $this->__("自定义内容") ?></li></ul></li></ul>');
```
注意: 如果页面没有#messages_product_view的话,就在想要显示的地方加上,如下:

```
<div id="messages_product_view"></div>
```