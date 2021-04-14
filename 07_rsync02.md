# 遠端備份
* rsync -avz /home/user/a vuser1@[ip]::mod1
* backup server上  
  * vim /etc/rsyncd.conf
```
uid = root
gid = root
pid file = /var/run/rsyncd.pid
lock file = /var/run/rsyncd.lock
log file = /var/run/rsyncd.log

[mod1]
path = /backup/centos
user chroot = no
max connection = 100
read only = false
hosts allow = 192.168.175.0/24
auth users = vuser1
secrets file = /backup/rsync.passwd
```
  * 在server端/backup 資料夾下
  * vim rsync.passwd
  * vuser1:123      [帳號]:[密碼]
  * killall rsync 刪除rsync daemon,只要有修改配置檔要重新啟動,重新啟動前要先刪除原本的Daemon
  * netstat -tunlp | grep 873 檢查rsync server是否有正常運作
  * rsync --daemon啟動
  * 
## 自動輸入密碼
* vim /etc/rsync.passwd
* 輸入密碼就可以
* 權限要更改為 600 chmod 600 /etc/rsync.passwd
* rsync -avz /home/user/a vuser1@[ip]::mod1 --password-file=/etc/rsync.passwd

