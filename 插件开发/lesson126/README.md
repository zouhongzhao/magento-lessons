# <<如何跳转到admin category的编辑页面(指定category id)>>

因为后台编辑某一个分类是通过点击后ajax生成的,url地址上没有category id。 默认为这种

```
http://test.com/index.php/admin/catalog_category/index/key/b40bed2f80a813e544e5851b6628a3dd/
```
如何跳转到具体分类的编辑页面呢。

控制器里面添加如下代码

```
function testAction(){
$categoryId = 10;
$this->_redirect('adminhtml/catalog_category/index', array('id'=>$categoryId, 'clear'=>1));
}
```
即可