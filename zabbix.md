# [zabbix](https://www.zabbix.com/download)
* yum install -y mariadb-server
* rpm -Uvh https://repo.zabbix.com/zabbix/4.0/rhel/7/x86_64/zabbix-release-4.0-2.el7.noarch.rpm
* yum clean-all
* yum install zabbix-server-mysql zabbix-web-mysql zabbix-agent
* yum install zabbix-get
* systemctl start mariadb
* systemctl enable mariadb
* mysql_secure_installation -> y -> n -> n -> y
* mysql -uroot -p 密碼設定1234
* create database zabbix character set utf8 collate utf8_bin;
* create user zabbix@localhost identified by 'zabbix';
* grant all privileges on zabbix.* to zabbix@localhost;
* quit;
* zcat /usr/share/doc/zabbix-server-mysql*/create.sql.gz | mysql -uzabbix -p zabbix
* vim /etc/zabbix/zabbix_server.conf
  * DBPassword=password
* vim /etc/httpd/conf.d/zabbix.conf
  * php_value date.timezone Asia/Taipei
* http://192.168.175.130/zabbix
* 設定密碼、資料庫名稱為zabbix
* 使用Admin/zabbix 登入



# 執行腳本的四種方法
* chmod +x 
  * 使用./來執行
  * 使用bash來執行
  * 使用source來執行.
  * 使用.空白建 來執行
  * 前兩者是在subprocess執行
  * 後兩者是在遠本的process執行
