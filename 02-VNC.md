# [vncserver1](https://codertw.com/%E4%BC%BA%E6%9C%8D%E5%99%A8/380173/) [vncserver2](https://www.footmark.info/linux/centos/centos7-vnc-install-setup/)
### 步驟
1. yum -y install tigervnc-server tigervnc
2. cat /etc/sysconfig/vncservers  尋找檔案位置
3. 複製一份檔案，並改名為vncserver@:1.service    
```
[root@localhost ~]# cp /lib/systemd/system/vncserver@.service /lib/systemd/system/vncserver@:1.service
```
4. vim /lib/systemd/system/vncserver@:1.service     並修改成這些內容
```
[Unit]
Description=Remote desktop service (VNC)
After=syslog.target network.target
[Service]
Type=forking
# Clean any existing files in /tmp/.X11-unix environment
ExecStartPre=/bin/sh -c '/usr/bin/vncserver -kill :1 > /dev/null 2>&1 || :'
ExecStart=/sbin/runuser -l root -c "/usr/bin/vncserver :1 -geometry 1280x720 -depth 24"
PIDFile=/root/.vnc/%H%i.pid
ExecStop=/bin/sh -c '/usr/bin/vncserver -kill :1 > /dev/null 2>&1 || :'
[Install]
WantedBy=multi-user.target
```
6. 更新systemctl
systemctl daemon-reload
7. 設定為自動啟動
systemctl enable vncserver@:1.service
8. 啟動vnc服務
systemctl start vncserver@:1.service
---
### 除錯
1. rm -R /tmp/.X11-unix/ 刪除裡面所有檔案
2. 設定 firewall 允許 vnc-server 的服務，並重新載入 firewall 設定（才能立即生效）
```
[root@localhost user]# firewall-cmd --permanent --add-service="vnc-server" --zone="public"
[root@localhost user]# firewall-cmd --reload
```
# 伺服器不能運作
1. 看是否正常運作    
systemctl status <伺服器名稱,ex sshd>
2. getenforce    看看是否為 Disabled    若不是，修改 /etc/selinux/config
```
SELINUX=disable
```
3. 看看防火牆關閉了沒    systemctl status firewalld
# 雜記
* vim內d d可以刪除整行
* [99.999% & 99.99%](https://en.wikipedia.org/wiki/High_availability#Percentage_calculation)

