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
* vim /etc/httpd/conf/httpd.conf
```
Alias /data /opt/docs
<Directory /opt/docs>
  Options Indexes       -->http://192.168.23.137/data/會出現docs資料夾下的東西
  Require all granted   -->http://192.168.23.137/data/docs.html
</Directory>
```
### 限制ip存取
* vim /etc/httpd/conf/httpd.conf
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
### 允許子目錄自訂選項
* AllowOverride All 允許子目錄自訂選項
* AllowOverride None 允許子目錄自訂選項
* AllowOverride 選項清單 AllowOverride Indexes []
### 增加帳號密碼驗證
* vim /etc/httpd/conf/httpd.conf
```
Alias /data /opt/docs
<Directory /opt/docs>
  Options Indexes
  Require all granted
  AllowOverride AuthConfig
  Order deny,allow
  Allow from 192.168.23.1
  Deny from all
</Directory>
```
* ls /opt/docs
* vim /opt/docs/.htaccess
```
AuthType Basic
AuthName "Private File Area"
AuthUserFile /opt/docs/.htpasswd
Require valid-user
```
* htpasswd -c .htpasswd [mary]
* systemctl restart httpd
### [虛擬主機](https://www.opencli.com/linux/rhel-centos-setup-apache-virtual-hosts)
* 先創立兩個document root
   * mkdir /var/www/website01.com
   * mkdir /var/www/website02.com
* 建立存放紀錄檔的目錄
   * mkdir /var/log/httpd/website02.com
   * mkdir /var/log/httpd/website01.com
* cd /var/www/website02.com
* vim index.php
```
<html>
    <head>
    <title>Welcome to <?php echo $_SERVER['HTTP_HOST']?></title>
    </head>
<body>
<?php echo $_SERVER['HTTP_HOST']?>
</body>
</html>
```
* cd ../website01.com/
* vim index.php 同上
* 調整擁有者chown -R apache:apache /var/www/website0*
* vi /etc/httpd/conf.d/website01.com.conf
```
<VirtualHost *:80>
    ServerName website01.com
    ServerAlias www.website01.com
    DocumentRoot /var/www/website01.com
    CustomLog /var/log/httpd/website01.com/access.log common
    ErrorLog /var/log/httpd/website01.com/error.log
</VirtualHost>
```
* vi /etc/httpd/conf.d/website02.com.conf
```
<VirtualHost *:80>
    ServerName website02.com
    ServerAlias www.website02.com
    DocumentRoot /var/www/website02.com
    CustomLog /var/log/httpd/website02.com/access.log common
    ErrorLog /var/log/httpd/website02.com/error.log
</VirtualHost>
```
* systemctl restart httpd
* 修改windows文件 C:\Windows\System32\drivers\etc 的host檔案
```
192.168.23.137    www.website01.com
192.168.23.137    www.website02.com
```
