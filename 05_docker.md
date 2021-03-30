# [docker](https://docs.docker.com/engine/install/centos/)
* 類似虛擬機
* 盡量輕量化、執行單一事項
* 持續執行某一任務，執行完就會關閉
* yum-config-manager     --add-repo     https://download.docker.com/linux/centos/docker-ce.repo
* yum install docker-ce docker-ce-cli containerd.io
* systemctl start docker
* docker pull [ubuntu:20.04] 下載鏡像
## httpd
* docker pull httpd
* docker run -itd -p 8080:80 httpd
* 在8080阜開docker的httpd server
* 打開電腦連192.168.175.128:8080

