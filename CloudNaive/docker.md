# DOCKER
## docker是什麼?
* 容器部屬的一個程式
### 何謂容器?
* 容器類似於虛擬機
   * 但比虛擬機輕量
   * 啟動快速


## run on centos 7
install docker```$yum install -y docker-io```   

default path ```/etc/sysconfig/docker```  

start docker service```$systemctl start docker```
## Basic operation
search image ```$docker search centos``` first image usually provided by official

pull ```$docker pull centos```

show images already install ```$docker images``` 

run ```$docker run centos```
* -d background 
* -P random map port
* -p assign port 91:80 91means machines port 80 means container's port
* -v /opt:/opt:ro src:dis ro means read only  
* -h hostname 
* --rm auto remove container after quit  
  
run and print hello world```$docker run centos /bin/echo 'Hello World'```

show container  ```$docker ps -a ```

```docker run --name mydocker -it centos /bin/bash```

若要看ip，ifconfig 是不可用的，要用 ip ad li

run container already build  ```$docker start CONTAINER ID```

stop container ```$docekr stop container id ```

remove container(container are not running) ```$docker rm container id ``` 

 run back ground```$docker attach container id```

 commit image ```$docker commit -m " " container id account/imagername:tag```

 ### Dockerfile (file name must be Dockerfile)
* FROM
* MAINTAINER
* RUN
* ADD
* WORKDIR
* VOLUME
* EXPOSE
* RUN
   
build ```$docker build -t name:version .```  "." means context
### resource 
* 隔離LXC Kernel namespace
   * Pid, Net, Ipc, Mnt, Uts, User
* restricton cgroup
   * CPU RAM
### net
* container veth to docker0(bridge)
* container veth to host
* container veth to dontainer
### registry
docker run -d -p 5001:5000 registry
### shipyard
* 圖形化
 ## linux 
 查看網橋 brctl show
show cpuinfo ```cat /proc/cpuinfo```
 ## 額外補充
 [變更image儲存碟](https://stevenjhu.com/2020/11/14/docker-docker-for-windows%E8%99%9B%E6%93%AC%E7%A3%81%E7%A2%9F%E5%AD%98%E6%94%BE%E4%BD%8D%E7%BD%AE%E7%A7%BB%E5%8B%95%E6%AD%A5%E9%A9%9F/)

 ## 參考連結
 [Docker — 从入门到实践](https://vuepress.mirror.docker-practice.com/)