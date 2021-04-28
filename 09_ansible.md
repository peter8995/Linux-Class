# [ansible](https://www.xuliangwei.com/oldxu/1247.html)
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
## script 模塊
* vim showip.sh
```
#!/usr/bin/bash

ip=`ifconfig ens33 | grep inet | grep -v inet6 | awk '{print $2}'`

echo $ip
```
* ansible webservers -m script -a "./showip.sh"
```
192.168.175.135 | CHANGED => {
    "changed": true,
    "rc": 0,
    "stderr": "Shared connection to 192.168.175.135 closed.\r\n",
    "stderr_lines": [
        "Shared connection to 192.168.175.135 closed."
    ],
    "stdout": "192.168.175.135\r\n",
    "stdout_lines": [
        "192.168.175.135"
    ]
}
192.168.175.134 | CHANGED => {
    "changed": true,
    "rc": 0,
    "stderr": "Shared connection to 192.168.175.134 closed.\r\n",
    "stderr_lines": [
        "Shared connection to 192.168.175.134 closed."
    ],
    "stdout": "192.168.175.134\r\n",
    "stdout_lines": [
        "192.168.175.134"
    ]
}
```
## copy 模塊
* ansible server2 -m copy -a "src=./httpd.conf dest=/tmp/a.conf"
```
[root@centos7-1 test-ansible]# ansible server2 -m copy -a "src=./httpd.conf dest=/tmp/a.conf"
192.168.175.134 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "checksum": "fdb1090d44c1980958ec96d3e2066b9a73bfda32",
    "dest": "/tmp/a.conf",
    "gid": 0,
    "group": "root",
    "md5sum": "f5e7449c0f17bc856e86011cb5d152ba",
    "mode": "0644",
    "owner": "root",
    "size": 11753,
    "src": "/root/.ansible/tmp/ansible-tmp-1619581230.44-5491-125369788383308/source",
    "state": "file",
    "uid": 0
}
```
* 備份ansible server2 -m copy -a "src=./httpd.conf dest=/tmp/a.conf backup=yes"
```
[root@centos7-1 test-ansible]# ansible server2 -m copy -a "src=./httpd.conf dest=/tmp/a.conf backup=yes"
192.168.175.134 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "backup_file": "/tmp/a.conf.5167.2021-04-28@11:42:25~",
    "changed": true,
    "checksum": "d9871913fc956d280523f4f1ed9fb6f71ca2f2dc",
    "dest": "/tmp/a.conf",
    "gid": 0,
    "group": "root",
    "md5sum": "9bad1f73aa65243ef2b68191954a45b2",
    "mode": "0644",
    "owner": "root",
    "size": 11755,
    "src": "/root/.ansible/tmp/ansible-tmp-1619581344.38-5555-77999032343133/source",
    "state": "file",
    "uid": 0
}
```
* 直接丟文字 ansible server2 -m copy -a "content='This is an apple.' dest=b.txt"
```
[root@centos7-1 test-ansible]# ansible server2 -m copy -a "content='This is an apple.' dest=b.txt"
192.168.175.134 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "checksum": "257e094683b7da61416ec54999394c4738f264f9",
    "dest": "./b.txt",
    "gid": 0,
    "group": "root",
    "md5sum": "b37820c8d302d977dfb0951e98716890",
    "mode": "0644",
    "owner": "root",
    "size": 17,
    "src": "/root/.ansible/tmp/ansible-tmp-1619581483.22-5602-190940625889186/source",
    "state": "file",
    "uid": 0
}
```
* 新增權限 擁有者 ansible server2 -m copy -a "content='This is an apple.' dest=b.txt owner=user group=user mode=666"
```
[root@centos7-1 test-ansible]# ansible server2 -m copy -a "content='This is an apple.' dest=b.txt owner=user group=user mode=666"
192.168.175.134 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "checksum": "257e094683b7da61416ec54999394c4738f264f9",
    "dest": "b.txt",
    "gid": 1000,
    "group": "user",
    "mode": "0666",
    "owner": "user",
    "path": "b.txt",
    "size": 17,
    "state": "file",
    "uid": 1000
}
```
## file模塊
* 創造一個aaa資料夾 ansible server2 -m file -a "path=/tmp/aaa state=directory"
```
[root@centos7-1 test-ansible]# ansible server2 -m file -a "path=/tmp/aaa state=directory"
192.168.175.134 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "gid": 0,
    "group": "root",
    "mode": "0755",
    "owner": "root",
    "path": "/tmp/aaa",
    "size": 6,
    "state": "directory",
    "uid": 0
}
```
* 


