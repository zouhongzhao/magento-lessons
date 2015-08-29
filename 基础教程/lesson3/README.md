# <<产品类型>>
Magento有六种产品类型：简单产品、分组产品、可配置产品、虚拟产品、捆绑产品和可下载产品，在添加产品时需要进行选择。

所以我们有必要清楚每一种类型的含义：

###简单产品(Simple Product)：

![](https://raw.githubusercontent.com/zouhongzhao/magento-lessons/master/基础教程/lesson3/simple-1.png)

我们先了解一下Simple Product，因为这是所有产品类型的基础类型。 
正如其名，Simple Product是网店上最简单的产品，通过店主定义Simple Product的一些属性，就可以创建个性化的产品了.

这类产品包含所有系统属性（因为所有类型的产品都有这些属性），你也可以给它添加Simple Attribute。 

比如一个简单Simple Product例子 – 西装。作为网店上面的一个独立的产品项目，店主可以为他定义颜色是黑色，有三个纽扣。那么当客户浏览网店的时候看到这件产品的颜色和纽扣的数量之后，他会决定是否购买这件西装。


###分组产品(Grouped Product)：

![](https://raw.githubusercontent.com/zouhongzhao/magento-lessons/master/基础教程/lesson3/group-1.png)

一样的理解，一个Grouped Product跟Configurable Product非常相似，也是一个可以在一个产品页面同时显示多个不同产品的功能。

然而，它是显示不同的方式（Fashion）。还是上面的例子，刚才新添加上去的那些西装不要，只是那件有三个纽扣的黑色西装。在卖这件西装的同时，你还在卖西裤和衬衫，看起来这三个产品是一套的。

仅使用Simple Product功能，每件产品都有单独的页面，共三个页面。但是，你比较有商业头脑，觉得这应该是一起销售的，因为它们是一套的。Grouped Product就可以很容易实现这个功能，只要创建一个Grouped Product（你可以为它起个名字，这里暂时叫“三件套”吧），并且你要把刚才的三个Simple Product跟这个关联。

现在在您的网站上，看看这个Grouped Product的页面，它分块地列出了这三个Simple Product的信息。这样做是提醒您的客户它们是一套的，而且鼓励他们一起购买这三件产品，不过它们还是可以选择购买单件。


###可配置的产品(Configurable Product)：

![](https://raw.githubusercontent.com/zouhongzhao/magento-lessons/master/基础教程/lesson3/configurable-1.png)

一个Configurable Product是一种给客户提供更多选择项的产品类型。
以为Simple Product是Configurable Product的基础，所以让我们继续用上面的西装作为例子。

除了上面提到那件西装外，你开始销售不同型号的西装：深蓝色的版本，以及一个有四个纽扣的版本（有黑色和深蓝色的）。
如果你只使用Simple Product功能的话，您需要创建三个新的Simple Product（深蓝色/3个按钮，黑色/4个按钮，以及深蓝色/ 4个按钮）。而且你有四个产品页面，每个有不同的颜色或纽扣。

如果使用Configurable Product，你就可以把这四个产品合成一个产品项，从而使您的客户通过选项保证找到他们喜欢的西装，同时使客户尽可能容易的浏览您的网店。一旦你已经创建了四个Simple西装，您可以创建一个Configurable西装，并且把四个Simple西装关联到Configurable西装上。在你创建Configurable西装的时候你不需要设置好颜色或纽扣数目，这些属性值都是在客户购买时自己选择的。这是一个可配置的产品，因为您的客户可以自己配置属性值。所以，你可以选择不在网站上显示四个Simple Product，而只显示一个Configurable Product。

当您的客户浏览这个产品时，会在产品信息下方看到两个下拉菜单，一个是颜色选择（有黑色和深蓝色选项），另一个是纽扣数量选择（有3和4选项）。
这些选项将决定他们选择了哪个对应的Simple Product。因为这些属性决定了哪个Simple Product，那么Magento就会在订单生成的时候关联到对应的Simple Product。

为什么这样做呢？因为Configurable Product并不是一个实在的产品，它只是一个可以在一个产品页面同时显示多个不同产品的功能而已。

这类产品在开发中遇到的几率最多,并且要注意的地方也最多

###虚拟的产品(Virtual Product)：

![](https://raw.githubusercontent.com/zouhongzhao/magento-lessons/master/基础教程/lesson3/virtual-1.png)

这是指一些无需库存和运输的商品，主要用于服务业。例如培训商品，销售课程，服务等；


###捆绑产品(Bundle Product)：

![](https://raw.githubusercontent.com/zouhongzhao/magento-lessons/master/基础教程/lesson3/bundle-1.png)

这是一种打包销售的模式，可以将数个商品打包一起销售；


###可下载产品(Downloadable Product)：

![](https://raw.githubusercontent.com/zouhongzhao/magento-lessons/master/基础教程/lesson3/downloadable-1.png)

主要用于软件，电子书等商品，你的商品可以上传到网站所在的服务器，也可以提供一个下载地址给顾客

###我们最常用的是简单产品和可配置产品。所以只要把这2种产品类型掌握就可以了。

###注意事项

在后台创建Configurable产品的时候必须先创建Configurable Attributes.不然的话就无法创建Configurable,因为这个Configurable Attributes是为他的子产品服务的。

比如遇到这种情况:
![](https://raw.githubusercontent.com/zouhongzhao/magento-lessons/master/基础教程/lesson3/configurable-2.png)

它提示必须要选择一个Configurable Attributes。

所以我们就去设置这个属性(Attribute),以属性size为例,假如这个可配置产品有2种尺寸的子产品(L和M).

![](https://raw.githubusercontent.com/zouhongzhao/magento-lessons/master/基础教程/lesson3/configurable-3.png)

设置好后，再创建Configurable产品的时候,就选择这个属性,然后就可以创建了。

![](https://raw.githubusercontent.com/zouhongzhao/magento-lessons/master/基础教程/lesson3/configurable-4.png)


