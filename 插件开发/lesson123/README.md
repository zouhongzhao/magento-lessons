#<<magento如何使用ftp上传下载>>

magento自带有ftp以及sftp类,直接调用即可,不需要另外写类。

```
<?php
class Iggo_Mbosa_Model_Cron_Ftp extends Iggo_Mbosa_Model_Cron {
	public $_ftp=null;
	public function getFtp(){
//先初始化ftp
		if(is_null($this->_ftp)){
			try {
				$this->_ftp = new Varien_Io_Ftp();
				$this->_ftp->open(
						array(
								'host'      => Mage::getStoreConfig('mbosa/autofutur/ftp_host'),
								'user'  => Mage::getStoreConfig('mbosa/autofutur/ftp_username'),
								'password'  => Mage::getStoreConfig('mbosa/autofutur/ftp_password'),
								'port'  => Mage::getStoreConfig('mbosa/autofutur/ftp_port'),
								'passive' => true
						)
				);
			} catch (Exception $e) {
				echo $e->getMessage();die;
			}
			
		}
		return $this->_ftp;
	}
	public function sendOrderToServer(){
//把export_dir里面的文件上传到ftp服务器里去
		$this->getFtp();
		$exportReadyDir = Mage::getStoreConfig('mbosa/autofutur/export_dir');
		$exportReadyDir = Mage::getBaseDir().DS.$exportReadyDir;
		$flocal = new Varien_Io_File();
		$flocal->open(array('path' => $exportReadyDir));
		if(!($fileList = $flocal->ls())){
			echo "export dir has no file!\r\n";
			return $this;
		}
		$total = count($fileList);
		
		$num = 0;
		echo "start..\r\n";
		foreach ($fileList as $item){
			$fileName = $item['text'];
			$fileType = $item['filetype'];
			$num ++;
			echo "{$fileName}: ({$num}/{$total}) ... ";
			if($fileType != 'ord'){
				echo "ignore!\r\n";
				continue;
			}
// 			$flocal->streamOpen($fileName, 'r');
			try{
				$_fileToExportRemoteTmp = $this->_ftp->write($fileName, $flocal->read($fileName));
				var_dump($_fileToExportRemoteTmp);
				if($_fileToExportRemoteTmp){
					$flocal->mv($fileName,$fileName.'.old');
					echo "ok!\r\n";
				}
			}catch(Exception $e){
				echo $e->getMessage();die;
			}
			
		}
		echo "over!\r\n";
		$this->_ftp->close();
	}
	
	public function downloadFromServer(){
//从ftp服务器里下载csv文件 更新产品的sku
				$this->getFtp();
        $dir = Mage::getBaseDir().DS.Mage::getStoreConfig('mbosa/autofutur/export_dir');
        $qtyFile = "tuotteet_vaiht.csv";
        $priceFile = "tuotteet.csv";
        $toDownload = array(
            "qty" => $qtyFile, 
            "price" => $priceFile
        );
        $qtyArr = array();
        $priceArr = array();
        $skuArr = array();
        foreach($toDownload as $type => $file){
            echo "Processing {$file} ... ";
            $_fileToImportRemoteTmp = $this->_ftp->read($file);
            if($_fileToImportRemoteTmp){
                $flocal = new Varien_Io_File();
                if ($flocal->write($dir.DS.$file, $_fileToImportRemoteTmp)) {
                    $flocal->open(array('path' => $dir));
                    $flocal->streamOpen($file, 'r');
                    while (false !== ($csvLine = $flocal->streamReadCsv(';','ISO-8859-1'))) {
                        if($type == "qty"){
                            if(isset($csvLine[3]) && !empty($csvLine[3])){
                                $csvLine[4] = (int)$csvLine[4] >0?(int)$csvLine[4]:0;
                                $sku = trim(trim(trim($csvLine[3]), '"'));
                                $qtyArr[$sku] = $csvLine[4];
                                array_push($skuArr, $sku);
                            }
                        }else{
                            if(isset($csvLine[8]) && !empty($csvLine[8])){
                                $price = str_replace(",", ".", $csvLine[3]);
                                $cost = str_replace(",", ".", $csvLine[4]);
                                $sku = trim(trim(trim($csvLine[8]), '"'));
                                $priceArr[$sku] = array(
                                    "price" => $price,
                                    "cost" => $cost
                                );
                            }
                        }
                    }
                    echo "DONE!\n";
                }
            }
        }
        $this->_ftp->close();
        
        $skuArr = array_unique($skuArr);
		if(!empty($skuArr)){
			echo "Processing qty&price ==============> \n";
			$productCollection = Mage::getModel('catalog/product')->getCollection()
									->addAttributeToSelect('sku_autofutur')
                                    ->addAttributeToSelect('qty')
                                    ->addAttributeToSelect('price')
                                    ->addAttributeToSelect('cost')
									->addAttributeToFilter('sku_autofutur',array('in'=> $skuArr))
                                    ->joinField('qty',
                                         'cataloginventory/stock_item',
                                         'qty',
                                         'product_id=entity_id',
                                         '{{table}}.stock_id=1',
                                         'left');
			$totalProduct = $productCollection->getSize();
			$count = 0;
			if($totalProduct > 0){
				foreach ($productCollection as $product){
					$count ++;
					echo "({$count}/{$totalProduct}) Pid: #{$product->getId()} [{$product->getSkuAutofutur()}]  ... ";
                    $setQty = false;
                    $setPrice = false;
                    $setCost = false;
                    
                    $oldQty = $product->getQty();
                    $oldPrice = $product->getPrice();
                    $oldCost = $product->getCost();
                    
                    $newQty = number_format($qtyArr[$product->getSkuAutofutur()], 4, ".", "");
                    $newPrice = false;
                    $newCost = false;
                    if(isset($priceArr[$product->getSkuAutofutur()])){
                        $newPrice = number_format($priceArr[$product->getSkuAutofutur()]["price"], 4, ".", "");
                        $newCost = number_format($priceArr[$product->getSkuAutofutur()]["cost"], 4, ".", "");
                    }
                    
                    if(!is_numeric($newQty) || $newQty < 0){
                        $newQty = 0;
                    }
                    
					echo "OLD-NEW QTY: [{$oldQty} - {$newQty}] ... ";
                    if($newQty != $oldQty){
                        $setQty = true;
                        echo "Need update!!! ... ";
                    }else{
                        echo "Ignore ... ";
                    }
                    
                    echo "OLD-NEW Price: [{$oldPrice} - {$newPrice}] ... ";
                    if($newPrice !== false && $oldPrice != $newPrice){
                        $setPrice = true;
                        echo "Need update!!! ... ";
                    }else{
                        echo "Ignore ... ";
                    }
                    
                    echo "OLD-NEW Cost: [{$oldCost} - {$newCost}] ... ";
                    if($newCost !== false && $oldCost != $newCost){
                        $setCost = true;
                        echo "Need update!!! ... ";
                    }else{
                        echo "Ignore ... ";
                    }
                    
                    if($setQty || $setPrice || $setCost){
                        echo "Saving ... ";
                        $_product = Mage::getModel('catalog/product')->load($product->getId());
                        if($setQty){
                            $stock_data=array(
                                'use_config_manage_stock' => 0,
                                'manage_stock' => 1,
                                'is_in_stock' => 1,
                                'qty' => $newQty
                            );
                            $_product->setData('stock_data',$stock_data);
                        }
                        
                        if($setPrice){
                            $_product->setData('price', $newPrice);
                        }
                        
                        if($setCost){
                            $_product->setData('cost', $newCost);
                        }
                        
                        try{
                            $_product->save();
                            echo "DONE\n";
                        }catch (Exception $exception) {
                            echo "Error: {$exception->getMessage()}\n";//die;
                        }
                    }else{
                        echo "Nothing need to update, continue.\n";
                    }
				}
			}
			echo "\n\n======FINISHED======\n\n";
		}
		
	}
}
```