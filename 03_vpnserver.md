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
