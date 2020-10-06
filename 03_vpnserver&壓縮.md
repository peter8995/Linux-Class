# [VPN server](https://exfast.me/2016/05/centos-install-7-x-vpn-pptp/)
1. 安裝yum第三方EPEL套件庫
```
yum install epel-release -y
```
2. 安裝PPTP
```
yum install ppp pptpd -y
```
3. vim /etc/pptpd.conf，拉到最下面新增
```
localip 10.0.10.1
remoteip 10.0.10.2-254
```
4. vim /etc/ppp/options.pptpd 搜尋ms-dns，將前面#去掉如下
```
ms-dns 8.8.8.8
ms-dns 8.8.4.4
```
5. 設定帳號密碼 vim /etc/ppp/chap-secrets
```
username pptpd password [指定IP、*不指定]
```
6. systemctl start pptpd

# 防火牆設定
* firewall-cmd --add-forward-port=port=1234:proto=tcp:toport=2222
  * 讓外部連進1234阜時轉移到2222阜
  
# 打包&壓縮
* 先把多個檔案打包，形成tar包
* 不同的壓縮技術
  * .tar.gz .tar.xz tar.bz2
* gz
```
[root@localhost ~]# ls
anaconda-ks.cfg  initial-setup-ks.cfg  testfile
[root@localhost ~]# gzip testfile
[root@localhost ~]# ls
anaconda-ks.cfg  initial-setup-ks.cfg  testfile.gz
[root@localhost ~]# gunzip testfile.gz
[root@localhost ~]# ls
anaconda-ks.cfg  initial-setup-ks.cfg  testfile
```
* zip
```
[root@localhost ~]# ls
anaconda-ks.cfg  initial-setup-ks.cfg  testfile
[root@localhost ~]# zip -r testfile.zip testfile
adding: testfile (stored 0%)
[root@localhost ~]# ls
anaconda-ks.cfg  initial-setup-ks.cfg  testfile  testfile.zip
[root@localhost ~]# rm testfile
rm: remove regular empty file ‘testfile’? y
[root@localhost ~]# unzip testfile.zip
Archive:  testfile.zip
 extracting: testfile
[root@localhost ~]# ls
anaconda-ks.cfg  initial-setup-ks.cfg  testfile  testfile.zip
```
* tar
  * c 產生新的包裹檔案
  * f 指定包裹檔案的名稱
  * v 觀看指令進度
  * x 解開tar包
  * t 列出tar包內的檔案清單
* 打包多個檔案
```
[root@localhost ~]# touch {a..c}
[root@localhost ~]# tar cfv tarall.tar a b c
a
b
c
```

* 打包資料夾
```
[root@localhost ~]# tar cfv mytmp.tar
tar: Cowardly refusing to create an empty archive
Try `tar --help' or `tar --usage' for more information.
[root@localhost ~]# tar cfv mytmp.tar /tmp
tar: Removing leading `/' from member names
/tmp/
/tmp/tracker-extract-files.1000/
/tmp/.esd-1000/
tar: /tmp/.esd-1000/socket: socket ignored
/tmp/ssh-7ZE9D5Br6eQa/
tar: /tmp/ssh-7ZE9D5Br6eQa/agent.1746: socket ignored
/tmp/systemd-private-e1436fe957234e4c8e15adb95fc9c83c-colord.service-mgMUPf/
/tmp/systemd-private-e1436fe957234e4c8e15adb95fc9c83c-colord.service-mgMUPf/tmp/
/tmp/systemd-private-e1436fe957234e4c8e15adb95fc9c83c-bolt.service-NO8dLp/
/tmp/systemd-private-e1436fe957234e4c8e15adb95fc9c83c-bolt.service-NO8dLp/tmp/
/tmp/.X0-lock
/tmp/systemd-private-e1436fe957234e4c8e15adb95fc9c83c-httpd.service-43qQz3/
/tmp/systemd-private-e1436fe957234e4c8e15adb95fc9c83c-httpd.service-43qQz3/tmp/
/tmp/systemd-private-e1436fe957234e4c8e15adb95fc9c83c-named.service-u3x0BP/
/tmp/systemd-private-e1436fe957234e4c8e15adb95fc9c83c-named.service-u3x0BP/tmp/
/tmp/systemd-private-e1436fe957234e4c8e15adb95fc9c83c-chronyd.service-sRclVK/
/tmp/systemd-private-e1436fe957234e4c8e15adb95fc9c83c-chronyd.service-sRclVK/tmp/
/tmp/systemd-private-e1436fe957234e4c8e15adb95fc9c83c-rtkit-daemon.service-IaWpXG/
/tmp/systemd-private-e1436fe957234e4c8e15adb95fc9c83c-rtkit-daemon.service-IaWpXG/tmp/
/tmp/.XIM-unix/
/tmp/.Test-unix/
/tmp/.X11-unix/
tar: /tmp/.X11-unix/X0: socket ignored
/tmp/.ICE-unix/
tar: /tmp/.ICE-unix/1881: socket ignored
tar: /tmp/.ICE-unix/1099: socket ignored
/tmp/.font-unix/
[root@localhost ~]# ls
a                b  initial-setup-ks.cfg  tarall.tar  testfile.zip
anaconda-ks.cfg  c  mytmp.tar             testfile
```

* tgz 簡化tar.gz的名稱
  * 壓縮指令 tar -czvf myfiles.tgz [file*]
  * 解壓縮指令 tar -xzvf myfiles.tgz

# 移除軟體
* rpm -e htppd
  * 可能會有相依性問題
* yum remove httpd

# 確認伺服器是否啟動
* systemctl status [httpd]
* netstat -tulnp | grep 

# 兩張網卡
* 有兩張網路卡，且本來同時可上網，其中一張不可上網之後，該如何做？
* 看路由表：ip route show
  * 把第一條(無法使用的網卡路由刪除)
  * ip route del default via [網卡ip]
