# ansible
* 三台電腦
* 2 3台只需要開啟ssh server
* 第一台 ssh-keygen
* ssh copy-id root@[二三台ip]
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
* 
