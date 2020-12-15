# php
* yum install php
* 安裝各種依賴包
    * yum -y install php-gd php-ldap php-odbc php-pear php-xml php-xmlrpc php-mbstring php-snmp php-soap curl curl-devel
* 在/var/www/html下生成一個php檔案，內容為 <?php  phpinfo(); ?>
## time.php
```<?php
header("Refresh:1");
date_default_timezone_set('Asia/Taipei');
$today = date('Y/m/d H:i:s');
echo $today;
?>```
