# php
* yum install php
* 安裝各種依賴包
    * yum -y install php-gd php-ldap php-odbc php-pear php-xml php-xmlrpc php-mbstring php-snmp php-soap curl curl-devel
* 在/var/www/html下生成一個php檔案，內容為 <?php  phpinfo(); ?>
## time.php
```
<?php
header("Refresh:1");
date_default_timezone_set('Asia/Taipei');
$today = date('Y/m/d H:i:s');
echo $today;
?>
```
## user 資料夾
* 在/home/user 下建立一個public_html的資料夾
* 設定權限 chmod 755 piblic_html
* 修改 /etc/httpd/conf.d/userdir.conf
```
UserDir enabled
 
    #
    # To enable requests to /~user/ to serve the user's public_html
    # directory, remove the "UserDir disabled" line above, and uncomment
    # the following line instead:
    #
    UserDir public_html
```
* systemctl restart httpd
* echo "hello" > /home/user/public_html/hello.html
* http://192.168.23.137/~user/hello.html
## data資料夾
* 在/opt資料夾下新增docs資料夾 mkdir /opt/docs
* echo "docs" > /docs/docs.html
* vim httpd.conf
```
Alias /data /opt/docs
<Directory /opt/docs>
  Options Indexes       -->http://192.168.23.137/data/會出現docs資料夾下的東西
  Require all granted   -->http://192.168.23.137/data/docs.html
</Directory>
```
### 限制ip存取
* vim httpd.conf
* 只有192.168.23.1不可存取
```
Alias /data /opt/docs
<Directory /opt/docs>
  Options Indexes
  Require all granted

  Order allow,deny
  Allow from all
  Deny from 192.168.23.1
</Directory>
```
* vim httpd.conf
* 只有192.168.23.1可存取
```
Alias /data /opt/docs
<Directory /opt/docs>
  Options Indexes
  Require all granted

  Order deny,allow
  Allow from 192.168.23.1
  Deny from all
</Directory>
```
