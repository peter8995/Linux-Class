# [docker](https://docs.docker.com/engine/install/centos/)
* 類似虛擬機
* 盡量輕量化、執行單一事項
* 持續執行某一任務，執行完就會關閉
* yum-config-manager     --add-repo     https://download.docker.com/linux/centos/docker-ce.repo
* yum install docker-ce docker-ce-cli containerd.io
* systemctl start docker
* docker pull [ubuntu:20.04] 下載鏡像
* docker images 查看已下載的鏡像
* docker rmi 移除鏡像
```
[root@localhost ~]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
ubuntu       20.04     8e428cff54c8   4 days ago    72.9MB
ubuntu       18.04     3339fde08fc3   4 days ago    63.3MB
httpd        latest    ae15ff2bdcb4   2 weeks ago   138MB
[root@localhost ~]# docker rmi 8e4
Untagged: ubuntu:20.04
Untagged: ubuntu@sha256:a15789d24a386e7487a407274b80095c329f89b1f830e8ac6a9323aa61803964
Deleted: sha256:8e428cff54c8a8d9759e0b0e13d9224ffe7617abb53a73e01550e32c15827e17
Deleted: sha256:b50dbc26e2451aefaf653957ffdde26b6b9050564adc2721b74fe371b875e4b9
Deleted: sha256:d3dd7d3e93adea03fa74e03d89b8f42ce37b7b626ef8ea3f0b72b2b96286f165
Deleted: sha256:0c78fac124da333b91fb282abf77d9421973e4a3289666bd439df08cbedfb9d6
[root@localhost ~]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
ubuntu       18.04     3339fde08fc3   4 days ago    63.3MB
httpd        latest    ae15ff2bdcb4   2 weeks ago   138MB
```
* docker ps 查看正在執行的docker
  * docker ps -a 包含已經死掉的docker
  * 若要完全移除 docker rm [id/names]
* docker stop [id/names]

## httpd
* docker pull httpd
* docker run -itd -p 8080:80 httpd
* 在8080阜開docker的httpd server
* 打開電腦連192.168.175.128:8080
## echo
```
[root@localhost ~]# docker run -it ubuntu echo "hi"
hi
[root@localhost ~]# docker run -it ubuntu:18.04 echo "hi"
hi
```
