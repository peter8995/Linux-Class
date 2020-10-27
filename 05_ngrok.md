# 軟體安裝
1. source code
2. rpm : dpkg
3. centos : yum    ubuntu : apt

### RPM是一個Redhat提出的一個套裝軟體標準
* 將編譯好的套裝軟體包裝好，可以直接安裝
* 可能有相依性的問題
* i386, i586,i686:32位元 x86_64:64位元 noarch:適用於所有平台
* rf:redhat fc:fedora cl:centos
* 可用來查詢(-q)query、安裝(-i)、升級(-u)、移除軟體(-e)
  * -qa 查詢所有已安裝套件
  * -qi 查詢特定套件的安裝資訊
  * -ql 查詢套件所安裝的檔案清單
  * -qf 查詢檔案來源軟體套件
