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
## [第二台主機](https://tw511.com/a/01/21076.html)
* rpm -Uvh https://repo.zabbix.com/zabbix/4.0/rhel/7/x86_64/zabbix-release-4.0-2.el7.noarch.rpm
* yum install -y zabbix-agent
* systemctl start zabbix-agent
* systemctl enable zabbix-agent
* vim /etc/zabbix/zabbix_agentd.conf
* 修改server ip 還有active server ip 為server id
* 修改hostname (自訂)
![image](https://user-images.githubusercontent.com/47874887/121304960-98d9f980-c92f-11eb-95cf-5f904dcb1a25.png)
![image](https://user-images.githubusercontent.com/47874887/121305091-bf983000-c92f-11eb-897c-a04c2917f19a.png)
* vim /etc/zabbix/zabbix_agentd.conf
* UserParameter=check_users, who | wc -l
* [root@centos7-1 home]# zabbix_get -s 192.168.175.135 -p 10050 -k "check_users"
1
* 回到網頁 configuration -> Hosts -> create item
![image](https://user-images.githubusercontent.com/47874887/121309337-a5ad1c00-c934-11eb-9eff-34d3752a6cc5.png)





# 執行腳本的四種方法
* chmod +x 
  * 使用./來執行
  * 使用bash來執行
  * 使用source來執行.
  * 使用.空白建 來執行
  * 前兩者是在subprocess執行
  * 後兩者是在遠本的process執行
