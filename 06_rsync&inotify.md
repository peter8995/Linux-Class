# rsync
## 使用模式
1. shell 模式：本地複製功能
2. 遠程shell功能：利用ssh實現數據的加密傳輸到遠程主機
3. 服務器模式：rsync工作在Daemon模式下
* 先建立免密碼ssh登入
* ssh-keygen 
* ssh-copy-id [第二台主機ip]
* ssh [ip] 嘗試是否成功
* 第二台同樣步驟
* 兩台皆用NAT
* 備分資料夾下的檔案
```
[root@localhost /]# mkdir /test -p
[root@localhost /]# mkdir /backup -p
[root@localhost /]# cd /test
[root@localhost test]# touch {1..5}.txt
[root@localhost test]# ls
1.txt  2.txt  3.txt  4.txt  5.txt
[root@localhost test]# rsync -avh /test/ /backup
sending incremental file list
./
1.txt
2.txt
3.txt
4.txt
5.txt

[root@localhost backup]# tree /backup/
/backup/
├── 1.txt
├── 2.txt
├── 3.txt
├── 4.txt
└── 5.txt

0 directories, 5 files
```
* 備份資料夾
```
[root@localhost test]# rsync -avh /test /backup
sending incremental file list
test/
test/1.txt
test/2.txt
test/3.txt
test/4.txt
test/5.txt
[root@localhost backup]# tree /backup/
/backup/
└── test
    ├── 1.txt
    ├── 2.txt
    ├── 3.txt
    ├── 4.txt
    └── 5.txt

1 directory, 5 files
```
* 修改已存在檔案內容或新增檔案不需要加--delete
* 刪除檔案或是檔案改名且要完全同步則要加上--delete
* 

