# 大量安裝
* 安裝新的一台虛擬機 ram:2048mb rom:50gb [bootorder:1.Network 2.Hard Disk](https://xx3d2ybnf.pixnet.net/blog/post/128713507) 網卡只用lansegment
* 一台已經安裝好之虛擬機 NAT HOSTONLY INTERNAL/ lan segment
* 利用 yum 安裝 tftp Server、DHCP Server、Vsftp Server 以及 syslinux 等套件：
```
yum install tftp-server dhcp syslinux vsftpd
```
* 設定 DHCP Server vim /etc/dhcp/dhcpd.conf
```
### 基本設定 ###
ddns-update-style interim;
ignore client-updates;
authoritative;
allow booting;
allow bootp;
allow unknown-clients;

#### 想要配發的網路 IP 位址 ###
subnet 192.168.100.0 netmask 255.255.254.0 {
  range 192.168.100.110 192.168.100.120;
# option domain-name-servers 8.8.8.8;
# option domain-name "example.com";
  option routers 192.168.100.254;
#  option broadcast-address 192.168.5.255; #not important
  default-lease-time 86400;
  max-lease-time 86400;
### PXE SERVER IP 位置 ###
  next-server 192.168.100.254; #  DHCP server ip
  filename "pxelinux.0";
}
```
* 然後去右上角設定內把lan segment網卡手動設定ip 192.168.100.254 netmask 24
* 修改 tftp 套件內容
```
[root@localhost ~]# cd /usr/lib/systemd/system
[root@localhost system]# vim tftp.service
[Unit]
Description=Tftp Server
Requires=tftp.socket
Documentation=man:in.tftpd

[Service]
ExecStart=/usr/sbin/in.tftpd -s /tftpboot  ##修改本行至合適的目錄
StandardInput=socket

[Install]
Also=tftp.socket
```
* 建立 tftpboot 目錄
```
mkdir /tftpboot
```
* 將必要的 syslinux 檔案，複製到 tftp Server 分享目錄內
```
cd /usr/share/syslinux
cp pxelinux.0 menu.c32 memdisk mboot.c32 chain.c32 /tftpboot/
chmod 644 -R /tftpboot
chmod 755 /tftpboot
```
* 在 tftp Server 目錄內，建立可提供 Linux 開機核心的目錄
```
mkdir /tftpboot/pxelinux.cfg
mkdir -p /tftpboot/netboot/
```
* 將iso檔案以及ks.cfg經由winScp複製到虛擬機中
* 將 Linux OS ISO 檔案內容掛載並複製到 vsftp 分享目錄上
```
mount (iso檔案) /mnt
cp -R /mnt/* /var/ftp/pub
```
* 將 Linux PXE 開機核心檔案，放置於 tftp Server 分享目錄上
```
cd /var/ftp/pub/images/pxeboot
cp vmlinuz initrd.img /tftpboot/netboot/
```
* 將 Linux Kstart 檔案，複製到/var/ftp/pub 目錄下
```
cp /root/anaconda-ks.cfg /var/ftp/pub/ks.cfg
chmod 644 /var/ftp/pub/ks.cfg
```
* 編寫 PXE Server 開機選單 vim /tftpboot/pxelinux.cfg/default
```
default menu.c32
prompt 0
timeout 30
MENU TITLE example.com PXE Menu
LABEL CentOS7_x64
MENU LABEL CentOS 7.2 X86_64
KERNEL /netboot/vmlinuz
APPEND initrd=/netboot/initrd.img inst.repo=ftp://192.168.5.104/pub ks=ftp://192.168.5.104/pub/ks.cfg
```
* 啟動各項服務
```
#systemctl enable vsftpd
#systemctl start vsftpd
#systemctl enable tftp
#systemctl start tftp
#systemctl daemon-reload
#systemctl enable dhcpd
#systemctl start dhcpd
```
