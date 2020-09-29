# 移除軟體
* rpm -e htppd
  * 可能會有相依性問題
* yum remove httpd

# 確認伺服器是否啟動
* systemctl status [httpd]
* netstat -tulnp | grep 

#
* 有兩張網路卡，且本來同時可上網，其中一張不可上網之後，該如何做？
* 看路由表：ip route show
  * 把第一條(無法使用的網卡路由刪除)
  * ip route del default via [網卡ip]
