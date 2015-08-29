#<<magento批量插入多行数据>>
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