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
* 
