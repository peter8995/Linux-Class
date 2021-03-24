# 智能DNS
* 看客戶端的ip來決定要給予哪條路徑(IP)
1. 新客戶可以給予一條需要經過許多檢查(ex.防火牆)的路徑，但是老客戶或是VIP可以走較快速的路徑
2. CDN(content delivery network)：假設目前此網站在美洲與亞洲各有一台雲端伺服器，智能DNS便可以依照客戶端的位置來給予伺服器的位置
## acl
* 將ip歸類到群組內
```
acl "env-test" {
   192.168.175.128;
};

acl "env-prod" {
   192.168.100.111;
};
```
* view 分配要給甚麼ip
```
view "env-test" {
        match-clients { "env-test"; };
        recursion yes;
        zone "a.com" IN {
          type master;
          file "env-test.a.com.zone";
        };
        zone "." IN {
          type hint;
          file "named.ca";
        };
};

view "env-prod" {
        match-clients { "env-prod"; };
        recursion yes;
        zone "a.com" IN {
          type master;
          file "env-prod.a.com.zone";
        };
        zone "." IN {
          type hint;
          file "named.ca";
        };
```
* 在/var/named下面建立env-prod.a.com.zone env-test.a.com.zone兩個檔案
```
[root@localhost named]# cat env-test.a.com.zone
$TTL 600 ;10 minutes

@ IN SOA       @ smallko.gmail.com (
               2021031803 ;serial
               10800      ;refresh
               900        ;retry
               604800     ;epxire
               86400      ;minimum
               )
@              NS    dns1.a.com.
@              NS    dns2.a.com.
dns.com.       A     192.168.56.108
dns1           A     192.168.56.108
dns2           A     192.168.56.109
www            A     192.168.56.100



[root@localhost named]# cat env-prod.a.com.zone
$TTL 600 ;10 minutes

@ IN SOA       @ smallko.gmail.com (
               2021031803 ;serial
               10800      ;refresh
               900        ;retry
               604800     ;epxire
               86400      ;minimum
               )
@              NS    dns1.a.com.
@              NS    dns2.a.com.
dns.com.       A     192.168.56.108
dns1           A     192.168.56.108
dns2           A     192.168.56.109
www            A     192.168.56.200
```
* 測試
  * 第一台電腦
```
[root@localhost named]# named-checkzone a.com /var/named/a.com.zone             
/var/named/a.com.zone:12: ignoring out-of-zone data (dns.com)
zone a.com/IN: loaded serial 2021031803
OK
[root@localhost named]# named-checkzone env-test.a.com /var/named/env-test.a.com.zone
/var/named/env-test.a.com.zone:12: ignoring out-of-zone data (dns.com)
zone env-test.a.com/IN: loaded serial 2021031803
OK
[root@localhost named]# named-checkzone env-prod.a.com /var/named/env-prod.a.com.zone
/var/named/env-prod.a.com.zone:12: ignoring out-of-zone data (dns.com)
zone env-prod.a.com/IN: loaded serial 2021031803
OK
[root@localhost named]# systemctl start named
[root@localhost named]# nslookup www.a.com 192.168.175.128
Server:         192.168.175.128
Address:        192.168.175.128#53

Name:   www.a.com
Address: 192.168.56.100
```
  * 第二台電腦
```
[root@localhost named]# systemctl start named
[root@localhost named]# nslookup www.a.com 192.168.175.128
Server:         192.168.175.128
Address:        192.168.175.128#53

Name:   www.a.com
Address: 192.168.56.200
```
