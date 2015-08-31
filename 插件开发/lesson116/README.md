# <<用zend mail发送邮件>>

因为magento是基于zendstudio的,所有我们就可以很方便的利用zend mail来发送邮件或者附件

```
$to_email = 'dev@fenzsoft.com';
		$to_name = 'Sisustalasilla';
		$subject = 'Packagedshipping rules Repeat';
		$Body= "Results: " . print_r( $weightStartArray, true );
		 
		$sender_email = "demo@hongzhao.com";
		$sender_name = "demo name";
		 
		$mail = new Zend_Mail('UTF-8');
		$mail->setBodyHtml($Body); //for sending message containing html code
		$mail->setFrom($sender_email, $sender_name);
		$mail->addTo($to_email, $to_name);
		//$mail->addCc($cc, $ccname);    //can set cc
		//$mail->addBCc($bcc, $bccname);    //can set bcc
		$mail->setSubject($subject);
                //添加附件
                //a,xml
		$at = new Zend_Mime_Part($content);
		$at->type        = 'application/xml';
		$at->disposition = Zend_Mime::DISPOSITION_INLINE;
		$at->encoding    = Zend_Mime::ENCODING_BASE64;
		$at->filename    = date('Y-m-d').'.xml';
		$mail->addAttachment($at);
                //b,csv
                 $at = new Zend_Mime_Part($content);
 
                 $at->type        = 'application/csv'; // if u have PDF then it would like -> 'application/pdf'
                 $at->disposition = Zend_Mime::DISPOSITION_INLINE;
                 $at->encoding    = Zend_Mime::ENCODING_8BIT;
                 $at->filename    = $filename;
                 $mail->addAttachment($at);
                //c,pdf
                 $pdf = $this->createPdf();
     
                 // attach PDF and set MIME type, encoding and disposition data to handle PDF files
                 $attachment = $mail->createAttachment($pdf);
                 $attachment->type = 'application/pdf';
                 $attachment->disposition = Zend_Mime::DISPOSITION_ATTACHMENT;
                 $attachment->encoding = Zend_Mime::ENCODING_BASE64;
                 $attachment->filename = 'document.pdf';
               //d,gif
                 $at = new Zend_Mime_Part($myImage);
                 $at->type        = 'image/gif';
                 $at->disposition = Zend_Mime::DISPOSITION_INLINE;
                 $at->encoding    = Zend_Mime::ENCODING_BASE64;
                 $at->filename    = 'test.gif';
 
                 $mail->addAttachment($at);
    
		$msg  ='';
		try {
			if($mail->send())
			{
				$msg = true;
			}
		}
		catch(Exception $ex) {
		
		}
```