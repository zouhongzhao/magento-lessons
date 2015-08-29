# <<adminhtml里的ajax用法>>

使用jquery写magento adminhtml的ajax post时,

假如call的controller为前台controller,一切照常,

但是假如call的是后台controller ,那么需要在url后面传递一个?isAjax=true

比如
```
jQuery.ajax({

url: url+”?isAjax=true”,

…
```

注意：

如果是用AjaxUpload上传的话,用上面的方法还是有错误.
```
”{“error”:true,”message”:“Invalid Form Key. Please refresh the page.”}”
```

需要设置一个form_key.如下

```
 var upload = new AjaxUpload('faarao_extcode_uploadfile_file', {
                        action: "<?php echo $this->getUploadUrl()?>"+"?isAjax=true",
                        name: 'csv_file',
                        onSubmit: function(file, ext){
                          ...
                                var FORM_KEY = '<?php echo $this->getFormKey() ?>';
								jQuery("#upload").html("common_uploading");
                                this.setData({form_key: FORM_KEY});
								this.disable();
							}
                        },
```

同理。如果用Form提交的话：
```
<form action="#" method="POST">
<input type="hidden" name="form_key" value="<? echo $this->getFormKey(); ?>" />
<input type="text" id="var1" name="var1" />
<input type="submit" id="submit" name="submit" />
</form>
```