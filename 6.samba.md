# SAMBA 
1. yum install samba -y
2. systemctl start smb
3. systemctl start nmb
4. smbpasswd -a user  用戶名需與系統內的對應
* vim /etc/samba/amb.conf
```
[tmp]
        comment = Temporary file space   //標示為暫時資料的資料夾
        path = /tmp   //路徑
        read only = No    //非唯讀
        public = Yes    //可共用
        browseable = Yes    //可瀏覽
```
# 檔案工具
## du
* -s 顯示目錄總用量
* -H 以磁碟單位顯示空間用量
* --max-deph=2 /home/user 顯示至/home/user下第二層目錄
## wc
* -l 顯示行數
* -c 顯示字元數
* -w 顯示英文字數
## tr
* -c或--complerment：取代所有不属于第一字符集的字符
* -d或--delete：删除所有属于第一字符集的字符
* -s或--squeeze-repeats：把連續重複的字符以單獨一個字符表示
* -t或--truncate-set1：先删除第一字符集较第二字符集多出的字符
* [:alnum:]：字母和數字
* [:alpha:]：字母
* [:cntrl:]：控制字符
* [:digit:]：數字
* [:graph:]：圖形字符
* [:lower:]：小寫字母
* [:print:]：可打印字符
* [:punct:]：標點符號
* [:space:]：空白字符
* [:upper:]：大寫字母
* [:xdigit:]：十六進制字符
* echo "ABcd123@" | tr [:upper:] [:lower:]  輸出abcd123@
* echo "ABcd123@" | tr -d "[:digit:]"  輸出ABcd@


# 雜記
* cat <<[EOF] >a  在輸入[]前 都會把東西寫入a
* curl --connect-timeout 3 www.google.com | wc -l 如果可以載入 回傳的就不會是0
