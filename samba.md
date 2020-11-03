# SAMBA 
1. yum install samba -y
2. systemctl start smb
3. systemctl start nmb
4. smbpasswd -a user  用戶名需與系統內的對應
```
[tmp]
        comment = Temporary file space   //標示為暫時資料的資料夾
        path = /tmp   //路徑
        read only = No    //非唯讀
        public = Yes    //可共用
        browseable = Yes    //可瀏覽
```
