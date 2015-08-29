# <<什么是属性>>
属性是magento非常重要的一个特性,是其精华所在。

在理解什么是属性之前,必须得先理解什么是EAV模型。

###什么是EAV模型

百度百科:
```
EAV(Entity–Attribute–Value,实体-属性-值)，一种数据库模型，使用eav建模的好处是可以动态为数据模型增加或移除属性。
传统的关系表模型将所有的属性组成为一张表的字段，这样的优点在于，数据的读取简单，然而，一旦出现业务的变更，属性的增加和删除，就需要对相应的数据表进行更改，不仅操作繁琐，并且有一定的风险。
EAV数据库模型将模型的属性以记录的形式存放在数据表中，即使需要新增和移除属性，仅仅需要删除数据表中相应的记录即可，而无需修改数据库结构。便于业务的扩展与灵活性的需要。
```
所以EAV是实体（Entity）、属性（Attribute）、值（Value）的意思，接下来来看看每一部分以便更好的理解它。
 
实体（Entity）
实体指的是magento的数据对象，如产品、分类目录、客户、订单等，每一个实体在数据库中都对应着一条实体记录。
 
属性（Attribute)
属性是指跟实体相关的一些性质数据，如产品实体有名称、价格、状态等。
 
值（Value）
值是最容易理解的了，就是指属性的值了。
 
###EAV是怎么工作的呢？

一直以来，数据库其实很简单的，比如我们现在要设计一个商城，需要有一张产品表，包括所有产品的信息，另一张表包括分类信息，也许还要一张表来连接这两张，这样很容易理解吧，然而magento却不一样，它跟产品以及分类有关的表有40多张，要想知道为什么，让我们来看下产品表。

![](https://raw.githubusercontent.com/zouhongzhao/magento-lessons/master/基础教程/lesson2/attribute-1.png)

 
不像其它的商城那样，所有的产品信息在一张表里，magento把产品信息分离在子表中，最顶上的表是catalog_product_entity，如果你看过这张表，你肯定发现了，它只包括产品的一些基础信息，除了SKU，其它你看不到任何有用的信息，幸运地是使用这张表你将可以从属性和值表中看到完整的产品记录。
 
让我们开始新建一条完整的产品记录，你需要将属性与我们的实体表相关联，做这之前先看下表eav_attribute，这张表在magento里为所有不同的实体存储了所有的属性，打开表，你会看到里面有好几百条不同属性的记录，为什么有些名称还是一样的呢？困惑吧？magento是如何辨别的呢？很快你就会注意到entity_type_id，每一个实体都会有一个entity_type_id，为了找出来，那就再回来catalog_product_entity表，看entity_type_id字段，你会发现所有的记录值都是10，如果你有去看catalog_category_entity，你将会看到一个不同的entity_type_id值。根据这个值和attribute code你就可以找到所有产品的属性，当然也可以所有其它实体的属性了。
 
思考下下面的查询：
```
# 找出所有产品的属性  
SELECT attribute_code FROM eav_attribute WHERE entity_type_id = 10;  


# 找出单个产品的属性   
SELECT attribute_code FROM eav_attribute WHERE entity_type_id = 10 AND attribute_code = 'name';   
```

你得到属性和实体了吧。接下来了解下值，值被分离在不同的表中，让我们看下所有前缀是catalog_product_entity的表，值是根据它们的类型来分的，例如，所有的价格以及其它decimal属性的会存储在表catalog_product_entity_decimal中，另外所有文本类型数据会存储在catalog_product_varchar中，需要指出的是每个属性存储的表，magento在eav_attribute表中使用字段backend_type记录，如果你运行以下查询，你将可以找到产品属性'name’的backend type。

```
SELECT attribute_code, backend_type FROM eav_attribute WHERE entity_type_id = 4 AND attribute_code = 'name';   
```
希望以上的查询返回的是varchar，这就是name的正确类型啦，基于以上，我们可以知道namer值被存储在表catalog_product_entity_varchar中，你认为下面的查询会存储在哪呢？想一想，然后copy it看下你对了没。

```
SELECT e.entity_id AS product_id, var.value AS product_name FROM catalog_product_entity e, eav_attribute eav, catalog_product_entity_varchar varWHERE e.entity_type_id = eav.entity_type_id AND eav.attribute_code = 'name' AND eav.attribute_id = var.attribute_id AND var.entity_id = e.entity_id  
```

###为什么使用EAV呢？
使用EAV是因为它相比其它普通的数据库结构要更容易扩展。开发者不用编辑核心数据库结构就可以为任何实体添加属性，并且当自定义的属性被添加后，不需要添加任何逻辑让magento保存它，因为这些在模型中都已经存在了，只要数据和属性创建后，模型就会保存了。
 
###EAV有什么缺点呢？
最主要的就是它的速度了，由于实体数据都是碎片式的，建立一个完整的实体记录需要许多表联合查询。幸运地是Magento团队开发的缓存系统相当强大(后面的课程会讲到它的缓存机制),所以速度这一块不必太操心。

由此可见,属性就相当于一个字段,但是比字段要复杂得多。

###如何添加属性,如何得到属性
目前最常用的是添加 category/product/customer/order 这4大模块的属性。因为这4个是核心。
至于怎么添加,后面会讲到的。
至于怎么得到,后面也会讲到。

