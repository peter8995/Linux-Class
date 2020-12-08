# FTP server
* yum install vsftpd
* 將/etc/vsftpd/vsftpd.conf內的
    * anonymous_enable=YES
    * write_enable=YES
    * anon_upload_enable=YES
* chown ftp:ftp pub 給權限
* prompt 在put/get的時候不會一直詢問是否上傳
* mput 批次上傳
```
ftp> mput b*
local: b1 remote: b1
227 Entering Passive Mode (192,168,23,137,230,153).
553 Could not create file.
local: b2 remote: b2
227 Entering Passive Mode (192,168,23,137,152,248).
150 Ok to send data.
226 Transfer complete.
local: b3 remote: b3
227 Entering Passive Mode (192,168,23,137,136,130).
150 Ok to send data.
```
ftp> mget a*
local: a1 remote: a1
227 Entering Passive Mode (192,168,23,137,117,227).
150 Opening BINARY mode data connection for a1 (0 bytes).
226 Transfer complete.
local: a2 remote: a2
227 Entering Passive Mode (192,168,23,137,35,23).
150 Opening BINARY mode data connection for a2 (0 bytes).
226 Transfer complete.
local: a3 remote: a3
227 Entering Passive Mode (192,168,23,137,56,113).
150 Opening BINARY mode data connection for a3 (0 bytes).
226 Transfer complete.
ftp> exit
221 Goodbye.
[root@localhost user]# ls
a1  a3  b2  Desktop    Downloads  myweb     Public     Videos
a2  b1  b3  Documents  Music      Pictures  Templates
```
