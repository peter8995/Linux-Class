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

* 用來監控 也可以監控cisco server
## 第二台主機
* rpm -Uvh https://repo.zabbix.com/zabbix/4.0/rhel/7/x86_64/zabbix-release-4.0-2.el7.noarch.rpm
* yum install -y zabbix-agent
* systemctl start zabbix-agent
* systemctl enable zabbix-agent
* vim /etc/zabbix/zabbix_agentd.conf
* 修改server ip 還有active server ip 為server id
* 修改hostname (自訂)
![image](https://user-images.githubusercontent.com/47874887/121304960-98d9f980-c92f-11eb-95cf-5f904dcb1a25.png)
![image](https://user-images.githubusercontent.com/47874887/121305091-bf983000-c92f-11eb-897c-a04c2917f19a.png)





# 執行腳本的四種方法
* chmod +x 
  * 使用./來執行
  * 使用bash來執行
  * 使用source來執行.
  * 使用.空白建 來執行
  * 前兩者是在subprocess執行
  * 後兩者是在遠本的process執行
