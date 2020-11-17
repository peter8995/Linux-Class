# [PentBox 蜜罐](https://it001.pixnet.net/blog/post/332731399-it%E5%B0%88%E9%A1%8C-%E5%9C%A8kali-%E4%B8%AD%E5%BB%BA%E7%BD%AE%E8%9C%9C%E7%BD%90%EF%BC%88pentbox%EF%BC%89)
* 紀錄被攻擊時系統所被做的事情，以便了解如何防禦。
* 先安裝[Ruby](https://tecadmin.net/install-ruby-latest-stable-centos/)
* git clone https://github.com/H4CK3RT3CH/pentbox-1.8.git
* ./pentbox.rb

# [seq](https://blog.csdn.net/qq_42935487/article/details/89028481)
* seq 起始值 [累加值] 結束值
* seq 1 12 
```
[root@localhost pentbox-1.8]# seq 1 12
1
2
3
4
5
6
7
8
9
10
11
12
```
* seq 1 2 11
```
[root@localhost ~]# seq 1 2 11
1
3
5
7
9
11
```
* echo {1..12..2}
```
[root@localhost ~]# echo {1..12..2}
1 3 5 7 9 11
```
* -w : seq -w 1 2 11
```
[root@localhost ~]# seq -w 1 2 11
01
03
05
07
09
11
```
* -s：指定分隔符，若沒指定就為\n seq -s "*" 1 5
```
[root@localhost ~]# seq -s "*" 1 5
1*2*3*4*5
```

# [sort](https://blog.csdn.net/qq_42935487/article/details/89028481)
```
[root@localhost pentbox-1.8]# cat a.sh
123 3
23 2
56 1
1 6

[root@localhost pentbox-1.8]# sort a.sh
123 3
1 6
23 2
56 1
```
* -n 依數字排列
```
[root@localhost pentbox-1.8]# sort -n a.sh
1 6
23 2
56 1
123 3
```
* -r 倒序
```
[root@localhost pentbox-1.8]# sort -nr a.sh
123 3
56 1
23 2
1 6
```
* -k
```
[root@localhost pentbox-1.8]# sort -n -k2 a.sh
56 1
23 2
123 3
1 6
```
* 與head awk合用
```
[root@localhost pentbox-1.8]# sort -n -k1 -r a.sh | head -1 | awk '{print $1}'
123
```
* -t 以某字符為分割符
```
[root@localhost pentbox-1.8]# cat a.sh
qq:aa:bb
ss:cc:vv
aa:bb:mm
dd:cc:bb
[root@localhost pentbox-1.8]# sort -t: -k2 a.sh
qq:aa:bb
aa:bb:mm
dd:cc:bb
ss:cc:vv
```


# 雜記
* bc
```
[root@localhost ~]# echo {1..10} | tr " " "+" | bc
55
[root@localhost ~]# seq -s "*" 1 5 | bc
120
```
