# <<Utilizing Magento notification system>>

```
Notice

Mage::getSingleton('core/session')->addNotice('Notice message');notice image

Success

Mage::getSingleton('core/session')->addSuccess('Success message'); success image

Error

Mage::getSingleton('core/session')->addError('Error message'); error image

Warning (admin only)

Mage::getSingleton('adminhtml/session')->addWarning('Warning message'); warning-image
```

参考:

http://inchoo.net/magento/magento-frontend/utilizing-magento-notification-system/