# [SSH Tunnel](http://csie.nqu.edu.tw/smallko/sdn/sshtunnel.htm)
## 簡易版
* 先用兩台虛擬機 使用Lan segment:a
* 將兩張lansegment網卡分別設為192.168.1.1 192.168.1.2 : ifconfig [網卡名稱] [ip]
* 互相ping
* 開啟第二台電腦的ssh server
* ssh -Nf -L 5555:192.168.1.2:80 user@192.168.1.2
* ssh -Nf -L [專接的阜號]:[sshserver ip] [帳號]@[ip]
## 透過sshtunnel連接到httpserver
* 準備三台電腦 第一台電腦和第二台有一個共同網域 第二台和第三台也有一個
* 第一台電腦設192.168.1.1 第二台與第一台共同網域的網卡設定192.168.1.2
* 第二台電腦與第三台電腦同一個網域的網卡設定192.168.2.2 第三台設定192.168.2.1
* 第二台電腦要打開ssh server
* 第三台電腦要有httpserver
* ssh -Nf -L 5557:192.168.2.1:80 user@192.168.1.2
## 簡易http server
* python -m SimpleHTTPServer
