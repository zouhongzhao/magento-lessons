# <<把atttibute从dropdown类型改成multiselect类型>>

主要是改数据表.因为dropdown和multiselect存的表不一样。

###目标:
 把产品属性brand.从dropdown类型改成multiselect类型

###步骤:

1,到eav_attribute表查找attribute_code为brand的数据,记录下它的attribute_id(比如为168)，entity_type_id(比如为4,一般4代表的是产品属性)。

2,更新属性类型(在配置表里)

```
UPDATE eav_attribute SET
entity_type_id = '4',
attribute_model = NULL,
backend_model = 'eav/entity_attribute_backend_array',
backend_type = 'varchar',
backend_table = NULL,
frontend_model = NULL,
frontend_input = 'multiselect',
frontend_class = NULL
WHERE attribute_id = '168';
``` 
3,把之前dropdown表里存的数据 全部 拷到multiselect表里

```
INSERT INTO catalog_product_entity_varchar ( entity_type_id, attribute_id, store_id, entity_id, value)
SELECT entity_type_id, attribute_id, store_id, entity_id, value
FROM catalog_product_entity_int
WHERE attribute_id = 168;
```

4,删除dropdown表里的旧数据

```
DELETE FROM catalog_product_entity_int
WHERE entity_type_id = 4 and attribute_id = 168;
```

5,把上面的代码整合到一起,放到脚本里或者phpmyadmin里去执行

大功告成,你打开后台产品编辑页面就会发现brand属性已经变成了多选框