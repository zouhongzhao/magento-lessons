# <<magento批量插入多行数据>>

为了减轻数据库压力和提高效率.一般都需要批量插入更新数据。

magento中主要用insertMultiple函数来完成

```
$table = Mage::getSingleton('core/resource')->getTableName('table_name');
$rows = array(
   array('cal_1'=>'value','cal_2'=>'value','cal_3'=>'value'),
   array('cal_1'=>'value','cal_2'=>'value','cal_3'=>'value')
);

public function insertRows($table,$rows)
{
   $write = Mage::getSingleton('core/resource')->getConnection('core_write');
   $write->insertMultiple($table,$rows);
}
```