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
## service模塊
* 可以開啟、重啟、關閉服務 類似systemctl
```

[root@centos7-1 test-ansible]# ansible server1 -m service -a "name=httpd state=restarted"
192.168.175.135 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "name": "httpd",
    "state": "started",
    "status": {
        "ActiveEnterTimestampMonotonic": "0",
        "ActiveExitTimestampMonotonic": "0",
        "ActiveState": "inactive",
        "After": "remote-fs.target -.mount system.slice systemd-journald.socket nss-lookup.target tmp.mount network.target basic.target",
        "AllowIsolate": "no",
        "AmbientCapabilities": "0",
        "AssertResult": "no",
        "AssertTimestampMonotonic": "0",
        "Before": "shutdown.target",
        "BlockIOAccounting": "no",
        "BlockIOWeight": "18446744073709551615",
        "CPUAccounting": "no",
        "CPUQuotaPerSecUSec": "infinity",
        "CPUSchedulingPolicy": "0",
        "CPUSchedulingPriority": "0",
        "CPUSchedulingResetOnFork": "no",
        "CPUShares": "18446744073709551615",
        "CanIsolate": "no",
        "CanReload": "yes",
        "CanStart": "yes",
        "CanStop": "yes",
        "CapabilityBoundingSet": "18446744073709551615",
        "CollectMode": "inactive",
        "ConditionResult": "no",
        "ConditionTimestampMonotonic": "0",
        "Conflicts": "shutdown.target",
        "ControlPID": "0",
        "DefaultDependencies": "yes",
        "Delegate": "no",
        "Description": "The Apache HTTP Server",
        "DevicePolicy": "auto",
        "Documentation": "man:httpd(8) man:apachectl(8)",
        "EnvironmentFile": "/etc/sysconfig/httpd (ignore_errors=no)",
        "ExecMainCode": "0",
        "ExecMainExitTimestampMonotonic": "0",
        "ExecMainPID": "0",
        "ExecMainStartTimestampMonotonic": "0",
        "ExecMainStatus": "0",
        "ExecReload": "{ path=/usr/sbin/httpd ; argv[]=/usr/sbin/httpd $OPTIONS -k graceful ; ignore_errors=no ; start_time=[n/a] ; stop_time=[n/a] ; pid=0 ; code=(null) ; status=0/0 }",
        "ExecStart": "{ path=/usr/sbin/httpd ; argv[]=/usr/sbin/httpd $OPTIONS -DFOREGROUND ; ignore_errors=no ; start_time=[n/a] ; stop_time=[n/a] ; pid=0 ; code=(null) ; status=0/0 }",
        "ExecStop": "{ path=/bin/kill ; argv[]=/bin/kill -WINCH ${MAINPID} ; ignore_errors=no ; start_time=[n/a] ; stop_time=[n/a] ; pid=0 ; code=(null) ; status=0/0 }",
        "FailureAction": "none",
        "FileDescriptorStoreMax": "0",
        "FragmentPath": "/usr/lib/systemd/system/httpd.service",
        "GuessMainPID": "yes",
        "IOScheduling": "0",
        "Id": "httpd.service",
        "IgnoreOnIsolate": "no",
        "IgnoreOnSnapshot": "no",
        "IgnoreSIGPIPE": "yes",
        "InactiveEnterTimestampMonotonic": "0",
        "InactiveExitTimestampMonotonic": "0",
        "JobTimeoutAction": "none",
        "JobTimeoutUSec": "0",
        "KillMode": "control-group",
        "KillSignal": "18",
        "LimitAS": "18446744073709551615",
        "LimitCORE": "18446744073709551615",
        "LimitCPU": "18446744073709551615",
        "LimitDATA": "18446744073709551615",
        "LimitFSIZE": "18446744073709551615",
        "LimitLOCKS": "18446744073709551615",
        "LimitMEMLOCK": "65536",
        "LimitMSGQUEUE": "819200",
        "LimitNICE": "0",
        "LimitNOFILE": "4096",
        "LimitNPROC": "7145",
        "LimitRSS": "18446744073709551615",
        "LimitRTPRIO": "0",
        "LimitRTTIME": "18446744073709551615",
        "LimitSIGPENDING": "7145",
        "LimitSTACK": "18446744073709551615",
        "LoadState": "loaded",
        "MainPID": "0",
        "MemoryAccounting": "no",
        "MemoryCurrent": "18446744073709551615",
        "MemoryLimit": "18446744073709551615",
        "MountFlags": "0",
        "Names": "httpd.service",
        "NeedDaemonReload": "no",
        "Nice": "0",
        "NoNewPrivileges": "no",
        "NonBlocking": "no",
        "NotifyAccess": "main",
        "OOMScoreAdjust": "0",
        "OnFailureJobMode": "replace",
        "PermissionsStartOnly": "no",
        "PrivateDevices": "no",
        "PrivateNetwork": "no",
        "PrivateTmp": "yes",
        "ProtectHome": "no",
        "ProtectSystem": "no",
        "RefuseManualStart": "no",
        "RefuseManualStop": "no",
        "RemainAfterExit": "no",
        "Requires": "system.slice -.mount basic.target",
        "RequiresMountsFor": "/var/tmp",
        "Restart": "no",
        "RestartUSec": "100ms",
        "Result": "success",
        "RootDirectoryStartOnly": "no",
        "RuntimeDirectoryMode": "0755",
        "SameProcessGroup": "no",
        "SecureBits": "0",
        "SendSIGHUP": "no",
        "SendSIGKILL": "yes",
        "Slice": "system.slice",
        "StandardError": "inherit",
        "StandardInput": "null",
        "StandardOutput": "journal",
        "StartLimitAction": "none",
        "StartLimitBurst": "5",
        "StartLimitInterval": "10000000",
        "StartupBlockIOWeight": "18446744073709551615",
        "StartupCPUShares": "18446744073709551615",
        "StatusErrno": "0",
        "StopWhenUnneeded": "no",
        "SubState": "dead",
        "SyslogLevelPrefix": "yes",
        "SyslogPriority": "30",
        "SystemCallErrorNumber": "0",
        "TTYReset": "no",
        "TTYVHangup": "no",
        "TTYVTDisallocate": "no",
        "TasksAccounting": "no",
        "TasksCurrent": "18446744073709551615",
        "TasksMax": "18446744073709551615",
        "TimeoutStartUSec": "1min 30s",
        "TimeoutStopUSec": "1min 30s",
        "TimerSlackNSec": "50000",
        "Transient": "no",
        "Type": "notify",
        "UMask": "0022",
        "UnitFilePreset": "disabled",
        "UnitFileState": "disabled",
        "WatchdogTimestampMonotonic": "0",
        "WatchdogUSec": "0"
    }
}
```
## group模塊
* 可以管理群組
```
[root@centos7-1 test-ansible]# ansible server1 -m group -a "name=sales gid=888"
192.168.175.135 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "gid": 888,
    "name": "sales",
    "state": "present",
    "system": false
}
```
## user模塊
* 可以管理用戶
```
[root@centos7-1 test-ansible]# ansible server1 -m user -a "name=tom uid=888 group=888 create_home=yes"
192.168.175.135 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "comment": "",
    "create_home": true,
    "group": 888,
    "home": "/home/tom",
    "name": "tom",
    "shell": "/bin/bash",
    "state": "present",
    "system": false,
    "uid": 888
}
```
## playbook
* vim playbook.yml
```
- hosts: server1
  tasks:
    - name: install httpd server
      yum: name=httpd state=present

    - name: configure httpd server
      copy: src=./httpd.conf dest=/etc/httpd/conf/httpd.conf

    - name: start httpd server
      service: name=httpd state=restarted enabled=yes
```
* ansible-playbook playbook.yml
```
[root@centos7-1 example1]# ansible-playbook playbook.yml
\
PLAY [server1] *****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [192.168.175.135]

TASK [install httpd server] ****************************************************
ok: [192.168.175.135]

TASK [configure httpd server] **************************************************
changed: [192.168.175.135]

TASK [start httpd server] ******************************************************
changed: [192.168.175.135]

PLAY RECAP *********************************************************************
192.168.175.135            : ok=4    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
### 加入handlers
```
- hosts: server1
  tasks:
    - name: install httpd server
      yum: name=httpd state=present

    - name: configure httpd server
      copy: src=./httpd.conf dest=/etc/httpd/conf/httpd.conf
      notify: restart httpd server

    - name: start httpd server
      service: name=httpd state=restarted enabled=yes

  handlers:
    - name: restart httpd server
      service: name=httpd state=restarted
```
* ansible-playbook playbook.yml
```
[root@centos7-1 example1]# ansible-playbook playbook.yml

PLAY [server1] *****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [192.168.175.135]

TASK [install httpd server] ****************************************************
ok: [192.168.175.135]

TASK [configure httpd server] **************************************************
changed: [192.168.175.135]

TASK [start httpd server] ******************************************************
changed: [192.168.175.135]

RUNNING HANDLER [restart httpd server] *****************************************
changed: [192.168.175.135]

PLAY RECAP *********************************************************************
192.168.175.135            : ok=5    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
### nfs server
* vim playbool.yml
```
- hosts: server1
  tasks:
    - name: install nfs server
      yum: name=nfs-utils state=present

    - name: configure nfs server step
      copy: src=./exports.j2 dest=/etc/exports backup=yes

    - name: create nfs group
      group: name=mynfs gid=666

    - name: create nfs user
      user: name=mynfs uid=666 group=666 shell=/sbin/nologin create_home=no

    - name: create nfs data
      file: path=/data state=directory owner=mynfs group=mynfs recurse=yes

    - name: start nfs server
      service: name=nfs state=restarted enabled=yes

- hosts: server1
  tasks:
    - name: install nfs software
      yum: name=nfs-utils state=present

    - name: client create nfs data
      file: path=/test-nfs state=directory

    - name: client mount nfs server
      mount:
        src: 192.168.175.135:/data
        path: /test-nfs
        fstype: nfs
        opts: defaults
        state: mounted
```
* vim exports.j2
```
/data/  192.168.175.0/24(rw,sync,no_root_squash,no_all_squash)
```
* ansible-playbook playbook.yml
* ![image](https://user-images.githubusercontent.com/47874887/118743861-cc23ee00-b885-11eb-9cc5-8bf5e2603d34.png)
### 變數
* vim playbook.yml
```
- hosts: server1
  vars_files: ./vars_public.yml

  tasks:
    - name: install {{ app1 }} and {{ app2 }}
      yum:
        name:
          - "{{ app1 }}"
          - "{{ app2 }}"
        state: present
```
* vim vars_public.yml
```
app1: wget
app2: gedit
```
* ansible-playbook playbook.yml
* vim playbook.yml
```
- hosts: 192.168.79.114
  tasks:
    - name: install {{ app1 }} and {{ app2 }}
      yum:
        name:
          - "{{ app1 }}"
          - "{{ app2 }}"
        state: present

- hosts: 192.168.79.115
  tasks:
    - name: install {{ app1 }} and {{ app2 }}
      yum:
        name:
          - "{{ app1 }}"
          - "{{ app2 }}"
        state: present
```
```
[root@centos7-1 example5]# tree
.
├── group_vars
├── host_vars
│   ├── 192.168.175.134
│   └── 192.168.175.135
└── playbook.yml
```
* 兩個ip檔案放入
```
app1: httpd
app2: vsftpd

app1: wget
app2: curl
```
* ansible-playbook playbook.yml
### 詳細資料
* vim playbook.yml
```
- hosts: server1
  tasks:
    - name: install httpd server
      yum: name=httpd state=present

    - name: configure httpd server
      copy: src=./httpd.conf dest=/etc/httpd/conf/httpd.conf
      notify: restart httpd server

    - name: start httpd server
      service: name=httpd state=started enabled=yes

    - name: Check httpd server
      shell: ps aux|grep httpd
      register: check_httpd
    
    - name: output variable
      debug:
        msg: "{{ check_httpd }}"

  handlers:
    - name: restart httpd server
      service: name=httpd state=restarted
```
```
[root@centos7-1 example6]# tree
.
├── httpd.conf
└── playbook.yml
```
* ansible-playbook playbook.yml
![image](https://user-images.githubusercontent.com/47874887/118752321-722b2480-b895-11eb-8fe3-eefd60e362c0.png)

