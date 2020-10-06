# [network file system](https://qizhanming.com/blog/2018/08/08/how-to-install-nfs-on-centos-7)
* 一個在UNIX上檔案分享的伺服器
### Server端
1. yum install nfs-utils -y
2. systemctl start rpcbind
3. systemctl start nfs
4. mkdir -p /data (-p可以忽略已存在檔案夾的錯誤)
5. chmod 755 /data 設置權限
6. vim /etc/exports
```
/data/     [ip](rw,sync,no_root_squash,no_all_squash)

/data: 共享資料夾位置
192.168.0.0/24: 客户端 IP 範圍 * 則代表沒有限制
rw: 權限
sync: 同步共享目錄
no_root_squash: 可以使用 root 權限。
no_all_squash: 可以使用普通用户權限。
```
7. 儲存後重啟 nfs server systemctl restart nfs
8. 檢查
```
[root@localhost user]# showmount -e localhost
Export list for localhost:
/data 192.168.23.0/24
```
### client端
1. yum install nfs-utils -y
2. systemctl start rpcbind
3. 檢查伺服器端共享目錄
```
[root@localhost user]# showmount -e 192.168.23.133
Export list for 192.168.23.133:
/data 192.168.23.0/24
```
4. mkdir -p /mydata
5. mount -t nfs 192.168.0.101:/data /mydata

要mount的時候不能在那個資料夾下面

### nfs & httpd server
1. 將 DocumentRoot 改到 /data
```
vim /etc/httpd/conf/httpd.conf
DocumentRoot "/data"
<Directory "/data">
    AllowOverride None
    # Allow open access:
    Require all granted
</Directory>
```
2. 重啟httpd server

