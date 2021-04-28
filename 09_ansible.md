# ansible
* 三台電腦
* 2 3台只需要開啟ssh server
* 第一台 ssh-keygen
* ssh-copy-id root@[二三台ip]
* 第一台 yum install -y ansible
* vim /etc/ansible/hosts
```
[webservers]
192.168.175.134
192.168.175.135

[server1]
192.168.175.135

[server2]
192.168.175.134
```
## command模塊
* ansible webservers -a "hostname"
```
[root@centos7-1 ~]# ansible webservers -a "hostname"
192.168.175.135 | CHANGED | rc=0 >>
centos7-3
192.168.175.134 | CHANGED | rc=0 >>
centos7-4
```
* ansible server1 -m command -a "hostname"
```
[root@centos7-1 ~]# ansible server1 -m command -a "hostname"
192.168.175.135 | CHANGED | rc=0 >>
centos7-3
```
* ansible server1 -m command -a "ifconfig"
```
[root@centos7-1 ~]# ansible server1 -m command -a "ifconfig"
192.168.175.135 | CHANGED | rc=0 >>
ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.175.135  netmask 255.255.255.0  broadcast 192.168.175.255
        inet6 fe80::20c:29ff:fea7:1ef  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:a7:01:ef  txqueuelen 1000  (Ethernet)
        RX packets 1191  bytes 629974 (615.2 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 660  bytes 81905 (79.9 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

ens37: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.2.1  netmask 255.255.255.0  broadcast 192.168.2.255
        inet6 fe80::a32e:f3b1:8b08:720c  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:a7:01:f9  txqueuelen 1000  (Ethernet)
        RX packets 18  bytes 3339 (3.2 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 35  bytes 4617 (4.5 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 32  bytes 2592 (2.5 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 32  bytes 2592 (2.5 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

virbr0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 192.168.122.1  netmask 255.255.255.0  broadcast 192.168.122.255
        ether 52:54:00:65:51:bf  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
* 較複雜之指令 例如管道 使用shell
```
[root@centos7-1 ~]# ansible server1 -m shell -a "netstat -tunlp | grep ssh"
192.168.175.135 | CHANGED | rc=0 >>
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      1551/sshd
tcp6       0      0 :::22                   :::*                    LISTEN      1551/sshd
```
## yum模塊
* ansible-doc yum 可以看解釋文件
* ansible server2 -m yum -a "name=httpd,tree state=latest" 安裝
* ansible server2 -m yum -a "name=httpd,tree state=abstnt" 解除安裝

