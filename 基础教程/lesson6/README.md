# <<添加产品>>

第3课中,讲了产品的类型。

除了可配置产品(Configurable Product有点特殊外),其他的产品添加方式都差不多。

所以本节课主要讲解simple product和Configurable Product的添加方法

###目标

一：创建一个simple product (test simple #1).归属于category #1分类。并且在前台分类页面显示出来。

二: 创建一个Configurable Product (test configurable #1).归属于category #1分类.并且在前台分类页面显示出来。

###步骤

Catalog -> Manage Products -> Add Product
####simple product
1,选择产品类型

![](https://raw.githubusercontent.com/zouhongzhao/magento-lessons/master/基础教程/lesson6/product-1.png)

```
Attribute Set: 后面讲属性的课程中会讲到,这是一个属性组。比如我销售电脑和鞋子,因为这2种类型的产品差别很大,如果用同一组属性的话,就会有很多属性用不到.所有就设2种属性组,合理利用。在这里,我们用default就ok了。
```

2,continue.需要注意的地方

```
SKU: 必填,仓库编号,唯一值,可以为数字或者字符串
Status: 必填,状态.要设为Enabled
Visibility: 必填,前台是否可见,设为'Catalog, Search'.意思是在分类页面和搜索页面都可见。
Tax Class: 必填,税率,国内的话一般设为none就行了,免税。
Qty: 必填，库存,要大于0
Stock Availability: 必填,库存状态,要设为In Stock
Websites: 必填,设为哪个网站显示
Categories: 必填,所属分类

prices: 必填,可以设置特价,组合价之类的
images: 非必填,上传图片.如果上次图片后,需要把Base Image/Small image/Thumbnall这3个下面的radio按钮选中,不然的话前台不会显示图片。
另外在ubutun下,有可能上次不了,因为是用flash上传插件做的，并且ubuntu下的flash不怎么友好.

Related Products: 非必填，关联产品,
  Upsells关联产品会显示在该页面的下方，表示与该产品相类似的一些产品，它们的质量更好，品牌更好，或者说能给商家带来更丰厚的利润的类似产品。如果顾客对该产品不满意，可以从这些相似产品中选择。例如客户在选购Thinkpad笔记本电脑，Upsells中一般应该显示其它笔记本电脑，T系列，X系列，又或者是戴尔的XPS系列等。
Up-sells: 非必填，关联产品
	Related products默认会显示在右侧边栏。这些产品被用来设计推荐给用户该产品之外的配套产品。比如用户在浏览笔记本电脑的时候，在右侧的Related products中可能会显示笔记本电脑鼠标，笔记本电脑屏幕保护膜等产品。
Cross-sells: 非必填,关联产品
   Crooss-sells产品会显示在结账时的购物车中。当用户添加产品到购物车后，准备结账时，都会进入购物车页面。Cross-sells关联的产品会显示在购物车页面下部的左侧。在Magento系统的设计中，Cross-sells与上面两个类型的关联产品完全不同。比如说用户购买笔记本电脑之后，进入到购物车页面，Cross-sells产品可能不会是与电脑相关的任何产品，而是杂志，糖果等其它主题的产品。

Product Reviews:非必填,产品的评论

Custom Options: 非必填,自定义的选项

```
![](https://raw.githubusercontent.com/zouhongzhao/magento-lessons/master/基础教程/lesson5/category-2.png)
![](https://raw.githubusercontent.com/zouhongzhao/magento-lessons/master/基础教程/lesson5/category-2.png)
![](https://raw.githubusercontent.com/zouhongzhao/magento-lessons/master/基础教程/lesson5/category-2.png)
![](https://raw.githubusercontent.com/zouhongzhao/magento-lessons/master/基础教程/lesson5/category-2.png)

3,保存,刷新前台分类页面,就会看到产品列表了
http://b2b.hongzhao.com/index.php/category-1.html





