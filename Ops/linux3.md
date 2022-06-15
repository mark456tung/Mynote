# 第一週筆記
## WabDAV
* epel用來安裝第三方軟體資料庫
* 如果有出現 下列三個則不需要再裝任何東西
    * dav_module (shared)
    * dav_fs_module (shared)
    * dav_lock_module (shared)
* 把權限給apache
    * chown -R apache:apache /var/www/html
```
su
yum install epel-relese
systemctl start httpd
systemctl status httpd
ifconfig
sudo httpd -M | grep dav
cd /var/www/html
mkdir webdav
chown -R apache:apache /var/www/html
chmod -R 755 /var/www/html
gedit /etc/httpd/conf.d/webdav.conf
systemctl restart httpd
systemctl status firewalld
getenforce
```
* gedit /etc/httpd/conf.d/webdav.conf 
```
DavLockDB /var/www/html/DavLock
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html/webdav/
    ErrorLog /var/log/httpd/error.log
    CustomLog /var/log/httpd/access.log combined
    Alias /webdav /var/www/html/webdav
    <Directory /var/www/html/webdav>
        DAV On
    #    AuthType Basic
    #    AuthName "webdav"
    #    AuthUserFile /etc/httpd/.htpasswd
    #    Require valid-user
    </Directory>
</VirtualHost>
```
* 回到電腦本機 -> 新增網路磁碟機 -> 在資料夾位置輸入 http:// IP位置 /webdav ->在案完成即可完成
* 測試webdav
    * 新增檔案或拖拉檔案到此磁碟機後，到 虛擬機 cd /webdav 用ls檢查

## IPV6連線
* 首先先到IPV6網站查一下有無IPV6
* [IPV6](https://test-ipv6.com/index.html.zh_TW)


* 虛擬機設定 -> 網路設為 橋接式的網路
* 如果要用IPV6去搜尋網址請用中括號刮起來
    * 如果埠號不再80的話在中括號後加上:埠號
* 申請網域名稱
    * [dynv6](https://dynv6.com/)
    * 如果IPV6跟虛擬機不一樣自行去修改
```
EX: http://[IPV6位置]/webdav/a.txt
```
* 

## 補充
* 正則表達式
    * -i是在這個文件進行操作，操作完資後寫入文件中(無-i不會修改內容)
    * s為取代的意思
    * ^ 以甚麼符號開頭(沒有寫則以任何開頭)、#& 取代  (&原本的內容)
    * /^/#&用後面#&的東西去取代^
    * / #皆可互換
```
 sed -i 's/^/#&/g' /etc/httpd/conf.d/welcome.conf
```
* 範例
    * 搜尋 Options Indexes FollowSymLinks 用 Options FollowSymLinks 內容取代
```
sudo sed -i "s/Options Indexes FollowSymLinks/Options FollowSymLinks/" /etc/httpd/conf/httpd.conf
``` 
## 參考資料
* [webdav](https://www.vultr.com/docs/how-to-setup-a-webdav-server-using-apache-on-centos-7/?fbclid=IwAR1RZt9cPFWrKZfjJOg4EJ6JSC7iF2i3PcAAZ6lMrsUhFmEmJoM-NuTDa2I)

# 第三週筆記
## Linux network namespace 原理與實踐
* ip netns add 創造一個空間
* ip netns ls查看有無創建
* ip netns del net1 刪除空間
* ip netns exec net1 ifconfig -a 看所有
* ip netns exec net1 ifconfig lo up 啟動
* veth pair對稱的虛擬網路卡



* ip link add type veth 建立網路卡
* ifconfig -a 剛該使創造網路卡預設是未開啟的
* ip netns exec net0 ip link set dev veth0 name eth0 把veth0改成eth0
* ip netns exec net0 ip addr add 10.0.1.1/24 brd + dev etho 配IP
```
ip netns add net0
ip netns ls
ip netns add net1
ip netns exec net1 ifconfig
ip netns exec net1 ip addr
ip netns exec net1 ifconfig lo up
ip link add type veth
ifconfig -a
ip link set dev veth0 netns net0
ip link set dev veth1 netns net1
ip netns exec net0 ifconfig -a
ip netns exec net1 ifconfig -a
ip netns exec net0 ip link set dev veth0 name eth0
ip netns exec net1 ip link set dev veth1 name eth0
ip netns exec net0 ip addr add 10.0.1.1/24 brd + dev etho
ip netns exec net0 ifconfig eth0 up
ip netns exec net1 ip addr add 10.0.1.2/24 brd + dev etho
ip netns exec net1 ifconfig eth0 up
ip netns exec net0 ping 10.0.1.2
開個新視窗
su
cd
ip netns exec net1 echo "hi" > hi.htm
ip netns exec net1 python -m SimpleHTTPServer 80
回原本視窗
ip netns exec net0 curl http://10.0.1.2/hi.htm
```
* ip netns exec net0 bash 開出一個殼
* su -user 切換使用者
* firefox 開啟火狐狸輸入10.0.1.2/hi.htm
```
ip netns exec net0 bash
su - user
firefox
```
* 多个不同 namespace 之间的通信
* brctl addbr br0創造橋接器



```
ip netns add net0
ip netns add net1
ip netns add net2
ip link add type veth
ip link add type veth
ip link add type veth
ip link set dev veth1 netns net0
ip link set dev veth3 netns net1
ip link set dev veth5 netns net2
ip netns exec net0 ip link set dev veth1 name eth0
ip netns exec net0 ip addr add 10.0.1.1/24 brd + dev etho
ip netns exec net0 ifconfig eth0 up
ip netns exec net0 ifconfig
ip netns exec net1 ip link set dev veth3 name eth0
ip netns exec net1 ip addr add 10.0.1.2/24 brd + dev etho
ip netns exec net1 ifconfig eth0 up
ip netns exec net1 ifconfig
ip netns exec net2 ip link set dev veth5 name eth0
ip netns exec net2 ip addr add 10.0.1.3/24 brd + dev etho
ip netns exec net2 ifconfig eth0 up
ip netns exec net2 ifconfig
yum install bridge-utils
brctl addbr br0
brctl addif br0 veth0
brctl addif br0 veth2
brctl addif br0 veth4
brctl show br0
ifconfig br0 up
ifconfig veth0 up
ifconfig veth2 up
ifconfig veth4 up
ip netns exec net0 ping 10.0.1.2
ip addr add 10.0.1.254/24 brd + dev br0
```
## docker
* 映像檔 Image
    * Docker 映像檔是一個模板，用來重複產生容器實體
* 容器 Container
    * 容器是用映像檔建立出來的執行實例，它可以被啟動、開始、停止、刪除。每個容器都是相互隔離、保證安全的平台。
* 倉庫 Repository
    * 倉庫（Repository）是集中存放映像檔檔案的場所
* docker用了兩種技術去完成
    * 隔離技術
    * cgroups
* docker的家目錄在 /var/liv/docker/
* 下載docker
    * docker -v 測試是否安裝成功 會出現Docker version
```
yum install -y yum-utils
 sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

sudo yum install docker-ce docker-ce-cli containerd.io

y

docker -v
```
* docker
    * docker ps可以看多少docker再跑 和docker id
    * 如果exit只是把狀態為run改為exit用docker ps -a可以看到
    * 第一種刪除用名稱 第二種用docker id(通常寫前三個字)
        * ex: docker rm e31 
```
docker images
docker run hello-world
docker images
docker run -it ubuntu echo "hi"
docker run -it ubuntu bash
docker run -it ubuntu:latest bash
docker ps
```
## 參考網址
* [详解云计算网络底层技术——Linux network namespace 原理与实践](https://segmentfault.com/a/1190000018391069?fbclid=IwAR3fZ4aXCqTjvZ-l8SsCd2UPuzuw6v8wRV_kB_n5yayenQn3zyQekOsDUao)
* [Docker 基礎教學與介紹 ](https://cwhu.medium.com/docker-tutorial-101-c3808b899ac6)

## 第四周筆記
## image
* image
    * image新增docker進行任何動作都會產生layer 如果沒有作儲存的動作產生的layer都會消失

新增hi.txt
```
docker run -it ubuntu bash
cd /home
ls
touch "hi" > hi.txt
cat hi.txt
exit
docker run -it ubuntu bash
cd /home
ls
```
* 保存docker的變更
    * 建立一個docker，建立一個檔案，ctrl+p + ctrl+q可在不結束的狀態退出
    * 使用docker ps確認剛建的CONTAINER ID
    * docker commit [CONTAINER ID] ubuntu:v1 會建立一個TAG為v1的新鏡像，可用docker images確認
        * [CONTAINER ID] 前三位就好
    * docker attach [CONTAINER ID]可以進入目前存在的鏡像
    * 如果要刪除 docker rm  [CONTAINER ID]
    * docker run -it ubuntu:v1 bash可以看到之前建的檔案被保存下來
```
docker run -it ubuntu bash
cd /home
ls
touch "hi" > hi.txt
ls
docker ps
docker commit [CONTAINER ID] ubuntu:v1
docker attach [CONTAINER ID]
docker rm  [CONTAINER ID]
docker run -it ubuntu:v1 bash
```
* docker 除錯
    * docker pull busybox sh 在這裡可以做ping 8.8.8 or ifconfig
```
docker pull busybox
docker pull busybox sh
```
* 如果docker正在執行還想回來做其他事
    * 如果用 docker attach 會回到相同的終端
    *  docker exec -it [CONTAINER ID] sh 可以開啟其他新的終端
```
docker exec -it [CONTAINER ID] sh 
```
* 如果再執行想刪除
    * 兩種
        * 先停止再刪除
        * 直接刪除    
```
docker stop [CONTAINER ID]
docker rm [CONTAINER ID]
```
```
docker rm -f $(docker ps -a -q)
```
* docker httpd


docker pull httpd

docker run -d -p 8889:80 httpd

which bash

gedit docker_httpd_setup5.sh 
```
#! /usr/bin/bash

for i in {1..5};
do
    portno=`expr 9000 + $i`
    docker run -d -p $portno:80 httpd
done
```
chomd + x docker httpd setup5.sh

./docker httpd setup5.sh

docker ps


```
* docker hub
```
docker login

輸入email 密碼

docker images

docker tfp ubuntu:v1 帳號/ubuntu:v1

docker push 帳號/ubuntu:v1

docker rmi -f [CONTAINER ID]

docker pull 帳號/ubuntu:v1

```
* docker mapping
    * docker apache的網頁主要儲存在/usr/local/apache2/htdocs#下
    * 在實體主機建立資料夾
    * docker run -d -p 8888:80 -v /home/user/web:/usr/local/apache2/htdocs httpd 把實體位置的資料夾和docker連接在一起
```
mkdir web

cd web

echo "hi" > hi.htm

ls

hi.htm

docker run -d -p 8888:80 -v /home/user/web:/usr/local/
apache2/htdocs httpd

```

# 第五週筆記
## 用腳本建立多的http伺服器與主機掛載
* 建立五個目錄
```
mkdir web1 web2 web3 web4 web5
cd web1
gedit hi.htm   內容為1
cd ..
cd web2
gedit hi.htm   內容為2
cd ..
cd web3
gedit hi.htm   內容為3
cd ..
cd web4
gedit hi.htm   內容為4
cd ..
cd web5
gedit hi.htm   內容為5
cd ..

* 腳本
    * 檔案位置在user下
    * -p如果不存在就去創 

gedit prepare_web.sh
```
#!/user/bin/bash

for i in {1..5};
do
    mkdir -p myweb$i
    cd myweb$i
    echo $i > hi.htm
    cd ..
done
```
chmod +x prepare_web.sh

./prepare_web.sh

ls

gedit docker_httpd_setup5.sh

```
#!/user/bin/bash
for i in {1..5};
do
    portno='expr 9000 + $i'
    docker run -d -p $portno:80 -v /home/user/myweb$i:/usr/local/apache2/htdocs httpd
done
```
./docker_httpd_setup5.sh
docker ps -a
最後去搜尋引擎輸入 IP:9001/hi.htm 看是否成功

## dockerfile
* dockerfile
    * 檔案位置在user下
    * FROM centos:centos7 以cemtos為基礎
    * RUN yum -y install httpd 鏡像安裝
    * .本地目錄  -t 標籤名稱

gedit Dockerfire
```
FROM centos:centos7
RUN yum -y install httpd
EXPOSE 80
ADD index.html /var/www/html
```
echo "hello world " > index.html

docker build -t myhttpd:1.0 .

docker images

docker run -d -p 8888:80 my httpd:1.0 /usr/sbin/

apachectl -DFOREGROUND

docker ps 檢查有無跑起來

## docker haproxy
* docker haproxy
    * 192.16.56.113:9001(此處填自己IP)
    * 如出現 is alreadyin use...就代表之前有使用過

gedit haproxy.cfg
```
defaults
    mode http
    timeout client 10s
    timeout commect 5s
    timeout server 10s
    timeout http-request 10s

frontend myfrontend
    bind 0.0.0.0:8080
    default_backend myservers

backend myservers
    balance roundrobin
    server server1 192.16.56.113:9001
    server server2 192.16.56.113:9002
    server server3 192.16.56.113:9003
    server server4 192.16.56.113:9004
    server server5 192.16.56.113:9005
```
docker run -p 8080:8080  -d --name haproxy-master -v /home/user/haproxy.cfg:/usr/local/etc/haproxy/haproxy.cfg --privileged=true haproxy

df5447cc3978605df9cfa1cbf0f924546f68c8389c4306b2fee66278ef5dc9d8
docker ps
最後去搜尋引擎輸入 IP:8080/hi.htm 看是否成功
```
## 用docker下載python3
* 到docker hub找python3
* 檔案位置在user下
```
docker pull faucet/python3

docker run -it -v /home/user:/tmp faucet/python3 bash

cd /mydata

python3 test.py

```
## AWK
* 檔案位置在user下
```
gedit file
```
11 dywang 81 12 A
152 linda 90 58 C
33 peter 72 95 C
4 rite 65 34 E
58 cora 5 85 D
```
gedit process.awk 
```
BEGIN{
  sum=0;
  i=0;
}
{
  sum+=$3;
  i++;
}
 
END { 
  printf("總和為:%d\n", sum);
  printf("average=%.2f\n", sum/i);
}
```
awk -f process.awk  file1

總和為:313

average=62.60

```
## 參考資料
* [awk](https://noootown.com/awk-useful-usage/)

# 第六週筆記
## rsync
* rsync
    * 可以將檔案或目錄從來源位置複製到目的位置
    * -v：verbose 模式，輸出比較詳細的訊息。 (互動可使用自動化少用)
    * -r：遞迴（recursive）備份所有子目錄下的目錄與檔案。
    * -a：封裝備份模式，相當於 -rlptgoD，遞迴備份所有子目錄下的目錄與檔案，保留連結檔、檔案的擁有者、群組、權限以及時間戳記。
    * -z：啟用壓縮。
    * -h：將數字以比較容易閱讀的格式輸出。
    * rpm -qa | grep rsync 有出現rsync代表有安裝反之則沒有安裝
    * rsync -avh --delete testdir/ /tmp 如果要刪則加上--delete 把tmp所有不同的刪掉
```
yum install rsync

rpm -qa | grep rsync

echo "hi" > a.txt

rsync -avh a.txt /tmp


開新視窗
cd /tmp
ls
cat a.txt

原視窗
mkdir testdir
cd testdir
touch {1..10}.txt
cd ..
rsync -avh testdir/ /tmp

新視窗
cd /tmp
ls

原視窗
cd testdir

echo "1" > 1.txt

cd ..

rsync -avh testdir/ /tmp

cd testdir

md5sum 1.txt

echo "11" > 1.txt

md5sum 1.txt

touch 11.txt 12.txt

cd ..

rsync -avh testdir/ /tmp

cd testdir

rm 4.txt 5.txt

cd ..

rsync -avh testdir/ /tmp

rsync -avh --delete testdir/ /tmp

cd testdir

mkdir a/b/c -p

tree

cd ..

rsync -avh --delete testdir/ /tmp


* 遠端備份
    * 若要使用其他rsync -avzh "ssh -p 22" testdor/ user@192.168.56.101:/tmp/backup/
    * ntpdate watch.stdtime.gov.tw同部台灣時間
    * #host1為客戶端 #host2為server
```
#host2
```
su 
cd /tmp
mkdir backup
```

#host1
```
ssh-keygen
ssh-copy-id user@192.168.56.101
ssh user@192.116.56.113
rsync -avzh testdor/ user@192.168.56.101:/tmp/backup/
touch testdir/111.txt
rsync -avzh testdor/ user@192.168.56.101:/tmp/backup/
```

#host2

```tree backup/```


#host1
```
cd testdir/
touch {a..c}.txt
cd ..
tree
rsync -avzh --excludr 'a.txt' --exclude'b.txt' testdor/ user@192.168.56.101:/tmp/
```

#host2
```
cd /etc

ls rsyncd.conf

gedit rsyncd.conf

```
uid=root
gid=root
pid file = /var/run/rsyncd.pid
log file = /var/log/rsyncd.log
secrets file = /etc/rsyncd.passwd

[mod1]
        path = /backup1
        read only = no
        auth users = vuser
```
gedit /etc/rsyncd.passwd
```
vuser:123456
```
chmod 600 /etc/rsyncd.passwd

ntpdate watch.stdtime.gov.tw

#host1

ntpdate watch.stdtime.gov.tw

#host2

systemctl start rsync

systemctl status rsync

cat /var/run/rsync.pid

cat /var/log/rsync.log

mkdir /backup1

#host1

gedit /etc/rsync_vuser.passwd
```
123456
```
chmod 600 /etc/rsync

chmod 600 /etc/rsync_vuser.passwd

rsync -avzu --progress --password-file=/etc/

rsync_vuser.passwd testdir/ vuser@192.156.56.101::mod1

## inotify
* m監測
* r子目錄有事件觸發
* q訊息顯示比較少

yum install inotify-tools

inotifywait -mrq /home/user/testdir --timefmt "%d-%m-%y %H %M"  --format "%T %w%f" -e create,modify,delete,move

#host2新視窗

cd testdir

rm 6.txt  

gedit backup.sh
```
#!/bin/bash
path=/home/user/testdir
server=192.168.56.111

inotifywait -mrq /home/user/testdir --timefmt "%d-%m-%y %H %M"  --format "%T %w%f" -e creat,emodify,delete,move | while read line
do
    if [ -f $line];then
    echo $line
    rsync -az --delete $line vuser@$server::mod1 --password-file=/etc/rsync vuser passed
    fi
done
```
./backup.sh


## 參考資料
* [rsync](https://blog.gtwang.org/linux/rsync-local-remote-file-synchronization-commands/)

# 第七週筆記
## Centos7編譯核心
* 編譯核心
    * uname -r 確認使用的版本
    * 到[kernel](https://www.kernel.org/)網站，選擇4.19.238的版本，右鍵複製tarball的鏈接 
    * 虛擬機wget+網址，虛擬機必須連網
    * 解壓縮tar xvjf linux-4.19.237.tar.xz
    * cd linux-4.19.237.tar.xz
    * cp -v /boot/config-3.10.0-1127.19.1.el7.x86_64 .config
    * make menuconfig會跳出圖形化介面，按save和exit
    * 依序執行make bzImage、make modules、make、makem odules_install、make install，注意此過程執行時間較長。如果發生錯誤執行make clean後再執行發生錯誤的步驟
    * grub2-mkconfig -o /boot/grub2/grub.cfg
    * 完成後重啟系統

```
uname -r
yum update
yum install -y ncurses-devel make gcc bc bison flex elfutils-libelf-devel openssl-devel grub2
wget https://cdn.kernel.org/pub/linux/kernel/v4.x/linux-4.19.238.tar.xz
tar xvjf linux-4.19.237.tar.xz
cd /boot
ll -h
cd
cd linux-4.19.237.tar.xz
cp -v /boot/config-3.10.0-1127.19.1.el7.x86_64 .config
make menuconfig
```
## jumpserver
* 安裝Docker Compose
```
curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose
chmod +x /usr/local/bin/docker-compose
systemctl status docker
docker-compose --version
```
* jumpserver
    * 如果出現error 用docker-compose down
        * docker ps -a 有些需要手動刪除
    * 如果部屬失敗，用docker -ps查看，把剛建立的容器刪掉
        * docker rm -f$(docker ps -a-q)

git clone --depth=1 https://github.com/wojiushixiaobai/Dockerfile.git
cd Dockerfile/
cp config_example.conf .env
gedit .env
```
# 版本号可以自己根据项目的版本修改
Version=v2.20.3

# 构建参数, 支持 amd64/arm64
TARGETARCH=amd64

# Compose
COMPOSE_PROJECT_NAME=jms
COMPOSE_HTTP_TIMEOUT=3600
DOCKER_CLIENT_TIMEOUT=3600
DOCKER_SUBNET=172.20.0.0/16

# 持久化存储
VOLUME_DIR=/opt/jumpserver

# MySQL
DB_HOST=mysql
DB_PORT=3306
DB_USER=root
DB_PASSWORD=nu4x599Wq7u0Bn8EABh3J91G
DB_NAME=jumpserver

# Redis
REDIS_HOST=redis
REDIS_PORT=6379
REDIS_PASSWORD=8URXPL2x3HZMi7xoGTdk3Upj

# Core
SECRET_KEY=B3f2w8P2PfxIAS7s4URrD9YmSbtqX4vXdPUL217kL9XPUOWrmy
BOOTSTRAP_TOKEN=7Q11Vz6R2J6BLAdO
DEBUG=FALSE
LOG_LEVEL=ERROR
```
```
docker-compose -f docker-compose-redis.yml -f docker-compose-mariadb.yml -f docker-compose.yml up
```
* 輸入iP就會到Jumpserver 預設帳號密:admin 
* users下的users->Create
* Assets
    * System Users ->create
        * ssh
        * Common user
                * name: user
                * username: user
            Admin user
            * name: root
            * username: user
    * Assets ->create
        * Hostname: centos7-1
        * IP: 192.168.56.101
        * Platform: Linux 
        * protocols ssh 22
        * Admin user:root
        * Nodes /Default


* 資產權限=> Permissions
    * Asset Permission->create
        * name: forTomCentos7-1
        * Asset centos7-1(192.168.56.101)
        * Nodes: /Default
        * system user :user(user)
        * Action:all
* 有兩種連線方式
    * 網頁版
         * 登出 登入tom帳號
         * view->workbench
         * web terminal
    * putty
        * ip 埠號2222 (jumpserver)
        * 帳tom密12456
        * s ->1
* 拒絕其他使用者危險動作
    * Users
        * Command Filters
            * Name:ForTom
            * user:tom
            * Asset :centos7-1(ip)
        * Command Filters rules
            * type :command
            * content: rm -f
            * Action :Deny
    

## 參考網址
* [CentOS 7 編譯核心](https://www.ltsplus.com/linux/rhel-centos-7-compile-kernel)
* [jumpserver](https://github.com/jumpserver/Dockerfile)

# 第九週筆記
## 用jumpserver寄信
* 步驟
    * 步驟1：進入 Google 安全設定頁面 
    * 步驟2：啟用「兩步驟驗證」 
    * 步驟3：設定「應用程式密碼」 
    * 步驟4：設定Google SMTP寄信 
        * SMTP主機：smtp.gmail.com
        * SMTP 埠號：465
        * SMTP 安全模式：SSL/TLS
        * SMTP 認證：是
        * SMTP 帳號：你的gmail帳號
        * SMTP 密碼：google應用程式密碼
```
 docker-compose -f docker-compose-redis.yml -f docker-compose-mariadb.yml -f docker-compose.yml up
```
* jumpserver 寄信
    * 登入jumpserver
    * system setting -> email -> STMP server
        * SMTP host：smtp.gmail.com
        * SMTP port：465
        * SMTP security mode：SSL/TLS
        * SMTP authentication：yes
        * SMTP account： [gmail信箱]
        * SMTP password：[google應用程式密碼]
    * 郵件服務器的SMTP帳號與發件人需一致
    * 確認結果
* 回到consoie
    * user 新增帳號
        * name:aa
        * username:aa 
        * 填入email
        * system roles :user
        * 其他皆不需要改 

## 文字三劍客
* grep用法
    *  -A<顯示行數>：除了顯示符合範本樣式的那一列之外，並顯示該行之後的內容。
    * -B<顯示行數>：除了顯示符合樣式的那一行之外，並顯示該行之前的內容。
    * -C<顯示行數>：除了顯示符合樣式的那一行之外，並顯示該行之前後的內容。
    * -c：統計匹配的行數
    * -e ：實現多個選項間的邏輯or 關係
    * -E：擴充套件的正則表達式
    * -f FILE：從FILE獲取PATTERN匹配
    * -F ：相當於fgrep
    * -i --ignore-case #忽略字元大小寫的差別。
    * -n：顯示匹配的行號
    * -o：僅顯示匹配到的字串
    * -q： 靜默模式，不輸出任何資訊
    * -s：不顯示錯誤資訊。
    * -v：顯示不被pattern 匹配到的行，相當於[^] 反向匹配
    * -w ：匹配 整個單詞
```
yum install httpd
systemctl start httpd
curl 127.0.0.1
cd /var/lod
cd httpd
more access_log
cat access_log | awk '{print $1}
cat access_log | awk '{print $1} |sort |uniq
cat access_log | awk '{print $1} |sort |uniq |wc -l
cat access_log | grep -c "[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}"
cd 
grep -r -n "root" /etc/
```
## sed
* sed用法
    * -n：不輸出模式空間內容到螢幕，即不自動列印，只列印匹配到的行
    * -e：多點編輯，對每行處理時，可以有多個Script
    * -f：把Script寫到檔案當中，在執行sed時-f 指定檔案路徑，如果是多個Script，換行寫
    * -r：支援擴充套件的正則表達式
    * -i：直接將處理的結果寫入檔案
    * -i.bak：在將處理的結果寫入檔案之前備份一份
    * 刪除空白行
        * sed -i "/^$/d" demo
## 額外資訊
* Asssets可以使用Export把資產轉換成csv檔案來看
## 參考資料
* [如何使用Google SMTP寄信(兩段式驗證+應用程式密碼) - 主機架站寄信教學](https://www.webdesigntooler.com/google-smtp-send-mail)
* [Linux文字三劍客超詳細教學---grep、sed、awk](https://tw511.com/a/01/11537.html)

# 第十一週
## 監控軟體
* 先改名子 
```
hostnamectl set-hostname centos7-1
```
* 綁定三台IP(記憶體至少2GB)
    * 右上角點開找到Wired Setting
    * 在Network 找到Wired下方齒輪點進去
    * IPV4 -> Manual -> 在Address地方輸入 ip ex:192.168.56.101 ->Netmask 255.255.255.0-> Gateway 輸入ip route show查尋到的
    * DNS 8.8.8.8
```
ip route show
```
* 三台修改hosts
    * 如果ping centos7-1 如果可行代表設置成功
    * 為了方便管理可以使用無密碼登入

gedit /etc/hosts
內容
```
127.0.0.1 localhost localhost.localdomin localhost4  localhost4.localdomin4
::1       localhost localhost.localdomin localhost6  localhost6.localdomin6
192.168.56.101 centos7-1
192.168.56.102 centos7-2
192.168.56.102 centos7-3
```
## prometheus



* 在第一台機器下載prometheus
    * 從user目錄 進入Downloads
    * 創造prometheus 群組
    * 創造prometheus
    * 去搜尋引擎輸入ip ex 192.168.56.101:9090


cd Downloads/

wget https://github.com/prometheus/prometheus/releases/download/v2.35.0/prometheus-2.35.0.linux-amd64.tar.gz

sudo groupadd prometheus

sudo useradd -g prometheus -m -d /var/lib/prometheus -s /sbin/nologin prometheus

mkdir -p /opt/module

tar xvfz prometheus-2.35.0.linux-amd64.tar.gz -C /opt/module

cd /opt/module/

chown -R prometheus.prometheus prometheus-2.35.0.linux-amd64

mv prometheus-2.35.0.linux-amd64/ prometheus

cd /usr/lib/systemd/system

gedit prometheus.service
```
[Unit]
Description=Prometheus
After=network-online.target

[Service]
Type=simple
Restart=on-failure
ExecStart=/opt/module/prometheus/prometheus --config.file=/opt/module/prometheus/prometheus.yml --storage.tsdb.path=/opt/module/prometheus/data --storage.tsdb.retention=30d --web.enable-lifecycle --log.level=debug

[Install]
WantedBy=multi-user.targe
```
systemctl daemon-reload

systemctl start prometheus

```
* 三台機器皆下載node_exporter
```
cd /home/user/Downloads/

wget https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-amd64.tar.gz

tar xvfz node_exporter-1.3.1.linux-amd64.tar.gz

mv node_exporter-1.3.1.linux-amd64 /opt/module/

mv node_exporter-1.3.1.linux-amd64 node_exporter

cd /usr/lib/systemd/system/

gedit node_exporter.service
```
[Unit]
Description=node_exporter
Documentation=https://github.com/prometheus/node_exporter
After=network.target
 
[Service]
Type=simple
ExecStart=/opt/module/node_exporter/node_exporter
Restart=on-failure

[Install]
WantedBy=multi-user.target
```
systemctl daemon-reload

systemctl start node_exporter

systemctl status node_exporter
```
* 編輯prometheus.yml
    * 編輯完後去windowsn搜尋 http://IP:9090/targets/target (ex: http://192.168.56.101:9090/targets/target) 
```
cd /opt/module/prometheus

gedit /prometheus.yml
在- targets: ['localhost:9090']下方加上
```
  - job_name: 'node_exporter1'
    static_configs:
     - targets: ['192.168.48.101:9100']

  - job_name: 'node_exporter2'
    static_configs:
     - targets: ['192.168.48.102:9100', '192.168.48.103:9100']
```
systemctl restart prometheus


## 參考資料
* [prometheus](https://iter01.com/515175.html)
* [Prometheus学习（二）](https://www.codeleading.com/article/10525895320/)
* [下載prometheus](https://prometheus.io/download/)

# 第十二周
## prometheus
* 先啟動第一台的prometheus  node_exporter
    * 另外兩台的 node_exporter
```
systemctl start prometheus

systemctl start node_exporter
```
* proqml
    * up{ instance=~"Ip:埠號*"} : =~政則表達式，.*:表示匹配全部!=~:正則表達式取反
    * graph: 圖形化介面
    * offset 5m 代表五分鐘前回推
```
 node_load1{instance="ip:埠號"} [1m] offset 5m  
```
* prometheus cpu模式
    * us：用户進程使用cpu的時間
    * sy：内核進程使用cpu的時間
    * ni：用户進程空間內改變過優先級的進程使用的cpu時間
    * id：空閒（没人用）的cpu時間
    * wa：等待io的cpu時間
    * hi：硬中断的cpu時間
    * si：軟中断的cpu時間
    * st：虛擬機管理程序使用的cpu時間

## Grafana(繪圖報紙)
* 開啟瀏覽器輸入http://IP:3000登入，預設的帳號密碼都是admin

```
cd /etc/yum.repos.d/

gedit grafana.repo
```
[grafana]
name = grafana
baseurl = https://packages.grafana.com/oss/rpm
repo_gpgcheck = 1
enabled = 1
gpgcheck = 1
gpgkey = http://packages.grafana.com/gpg.key
sslverify = 1
sslcacert = /etc/pki/tls/certs/ca-bundle.crt
```
yum install grafana

systemctl daemon-reload

systemctl start grafana-server

systemctl status grafana-server

```
## Grafana 和 Prometheus 聯繫
* 先點左側齒輪(configuration)
* 再點選Add data source



* 點選Prometheus



* 在http 的URL 輸入http://192.168.56.101:9090(其他皆為預設)
* 點選 save&test 確定是否成功
* 至windows瀏覽器輸入[Grafana](https://grafana.com/) -> Products -> Dashboards -> Node Exporter Full
* 回到http://IP:3000 點選 Create->import 在import via grafana.com 那欄輸入1860 按 Load
* Prometheus選擇來源Prometheus 再點選import就會出現面板

## pushgateway
* 最後在windows瀏覽器輸入http://IP:9090，就可以看到pushgateway
```
cd /home/user/Downloads/

wget https://github.com/prometheus/pushgateway/releases/download/v1.4.2/pushgateway-1.4.2.linux-amd64.tar.gz

mv pushgateway-1.4.2.linux-amd64.tar.gz pushgateway

mv pushgateway /opt/module/

cd /opt/module/

gedit /usr/lib/systemd/system/pushgateway.service
```
Description=pushgateway
After=network.target

[Service]
Type=simple
ExecStart=/opt/module/pushgateway/pushgateway 
Restart=on-failure

[Install]
WantedBy=multi-user.target
```
systemctl start pushgateway

systemctl status pushgateway

gedit /opt/module/prometheus/prometheus.yml
```
#最下方加上
- job_name: 'pushgateway'
    honor_labels: true
    static_configs:
      - targets: ['192.168.56.101:9091']

systemctl daemon-reload

systemctl restart prometheus

## 參考資料
* [Prometheus 查询语言 PromQL 的 CPU 使用率计算方法](https://blog.csdn.net/qq_35753140/article/details/105121525)
* [Prometheus监控CPU,Memory,Disk](https://www.modb.pro/db/43636)
* [在 CentOS 7 上安裝 Grafana 6.0.2](http://max043.blogspot.com/2019/03/centos-7-grafana-602.html)

# 第十三周
## prometheus監控httpd

host1

systemctl start prometheus

systemctl start node_exporter

systemctl start pushgateway

host2

systemctl start node_exporter

host3

systemctl start node_exporter

host2

echo "some_metric 6.66" | curl --data-binary @- http://192.168.56.101:9091/metrics/job/some_job

mkdir -p /app/scripts/pushgateway

cd /app/scripts/pushgateway

gedit tcp_waiting_connection.sh
```
#!/bin/bash

# For waiting connections
label="count_netstat_wait_connetions"
count_netstat_wait_connetions=`netstat -an | grep -i wait | wc -l`
echo "$label:$count_netstat_wait_connetions"
echo "$label $count_netstat_wait_connetions" | curl --data-binary @- http://192.168.56.101:9091/metrics/job/pushgateway/instance/$instance_name
```
./tcp_waiting_connection.sh

yum install -y httpd

systemctl start httpd

curl http://192.168.56.101 > /dev/null

echo $?

cp tcp_waiting_connection.sh 

check_httpd_centos7-2.sh

gedit check_httpd_centos7-2.sh
```
#!/bin/bash

# For waiting connections
label="httpd_status_centos7_2"
curl -q  http://192.168.157.136 >/dev/null 2>&1
status=`echo $?`
if [ $status -eq 0 ];then
   result=1
else
   result=0
fi
#echo $label $result
echo "$label $result" | curl --data-binary @- http://192.168.56.101:9091/metrics/job/$label
```
./check_httpd_centos7-2.sh
systemctl start httpd
./check_httpd_centos7-2.sh
crontab -e
```
* * * * * /app/scripts/pushgateway/check_httpd_centos7-2.sh
* * * * * sleep 15;/app/scripts/pushgateway/check_httpd_centos7-2.sh
* * * * * sleep 30;/app/scripts/pushgateway/check_httpd_centos7-2.sh
* * * * * sleep 45;/app/scripts/pushgateway/check_httpd_centos7-2.sh
```
crontab -l

## Grafana繪圖
* 左側+的符號選到Dashboard -> add a new panel
* 找到 Data source選擇 Prometheus


* 找到query的Meteics browser 輸入 httpd_status_centos7_2 在案Run query 



* 點選save進行儲存 就可形成圖表

#  第十四週
## 先啟動prometheus grafana
```
host1
systemctl start prometheus
systemctl start node_exporter
systemctl start pushgateway
host2
systemctl start node_exporter
host3
systemctl start node_exporter
host1
systemctl start docker
docker run -d --name=grafana -p 3000:3000 grafana:7.5.15
```
* 開啟瀏覽器輸入http://IP:3000登入，預設的帳號密碼都是admin
* 左側齒輪(configuration)找到 Data source選擇 
* 再點選Add data source

* 點選Prometheus




* 在http 的URL 輸入http://192.168.56.101:9090(其他皆為預設)
* 點選 save&test 確定是否成功
* 至windows瀏覽器輸入[Grafana](https://grafana.com/) -> Products -> Dashboards -> Node Exporter Full
* 回到http://IP:3000 點選 Create->import 在import via grafana.com 那欄輸入1860 按 Load
* Prometheus選擇來源Prometheus 再點選import就會出現面板

* 左側+的符號選到Dashboard -> add a new panel
* 找到 Data source選擇 Prometheus如下圖



* 找到query的Meteics browser 輸入 httpd_status_centos7_2 在案Run query 


* 點選save進行儲存 就可形成圖表

## Grafana警報與line連結
* 先登入[line](https://notify-bot.line.me/zh_TW/)
* 創建群組 -> 邀請好友(Line Notify)
* 在網頁的line找到 -> 管理登入服務 -> 發行權杖 -> 複製權杖 
* 左側到鈴鐺圖示(Alerting) 選擇 Notification channels -> add channel->type 下拉選單選擇 LINE 把權杖輸入至Token name 輸入 LineNotify ->  按test 看是否跳出訊息 跳出則為成功


* 按 search 找到 剛才創建的面板 點選進入 後再點選Edit 


* 進入Edit後 找到Alert -> Create Alert
* WHEN 選擇last() OF query(A,10s,now) IS BELOW 0.9  
* Message 選擇 LineNotify 


## Ansible
* 先做無密碼登入，方便管理及維護
* 只有主控端需要安裝Asible
* 另外兩台要先確保sshd有開啟
*  ansible-doc -l 列出所有模組列表
* -v	詳細模式，如果執行成功，輸出詳細結果
* -i	指定host檔案路徑，預設在/etc/ansible/hosts
* -f,-forks=NUM	NUM預設是整數5，指定fork開啟同步程序的個數
* -m	指定使用的module名稱，預設command模組
* -a	指定模組的引數
* -k	提示輸入SSH密碼，而不是使用基於ssh金鑰認證
* -sudo	指定使用sudo獲取root許可權
* -K	提示輸入sudo密碼
* -u	指定行動端的執行使用者
* -C	測試命令執行會改變什麼內容，不會真正的去執行
* ansible servers -m command -a "date" 時間同步

yum install epel-release -y
yum install ansible

host2.3

systemctl status sshd

host1

gedit /etc/ansible/hosts

```
[server1]
192.168.56.102

[server2]
192.168.56.103

[servers]
192.168.56.102
192.168.56.103
```
ansible server1 -m ping

ansible server2 -m ping

ansible servers -m ping

ansible-doc -l

host3

date -s 20220101

host1

ansible servers -m command -a "date"



# 第十五周
## Ansible
* ansible servers -m ping 先確定可以執行
* uptime 顯示開機開多久了
* 檢查節點的記憶體情況
* command
    * 想要進入資料夾執行 用chdir
    * creates=a ls 如果a存在就不會執行ls
    * removes=a ls 如果a存在才執行ls
* file
    * absent刪除
    * directory創建
* yum
    * 安裝
        *  latest 
        * present
        * installed
    * 解除安裝
        * removed
        * absent

```
ansible servers -m ping
ansible servers -m command -a "uptime"
ansible -m command -a "free -m" server1
ansible server1 -m command -a "useradd tom"
ansible server1 -m command -a "id tom"
ansible server1 -m shell -a "echo tom > tom.txt"
ansible server1 -m shell -a "cat tom.txt"
mkdir test-ansible
cd test-ansible/
ansible server1 -m command -a "which bash"

gedit a.sh
```
#!/usr/bin/bash
date
hostname
echo "hello"
```
chmod +x a.sh
./a.sh
```
ansible server1 -m script -a "./a.sh"
ansible server1 -m script -a "/root/test-ansible/a.sh"
ansible server1 -m command -a "chdir=/tmp ls"
ansible server1 -m command -a "creates=a ls"
ansible server1 -m command -a "removes=a ls"
ansible server1 -m command -a "removes=a touch a"
echo "hello" > hello.txt
ansible server1 -m copy -a "src=./hello.txt dest=/tmp/hello.txt owner=user group=user mode=0644"
ansible server1 -m command -a "ls -l /tmp/hello.txt"
ansible server1 -m copy -a "content'hello world' dest=/tmp/hi.txt"
ansible server1 -m command -a "cat /tmp/hi.txt"
ansible server1 -m copy -a "content'hello world123 world' dest=/tmp/hi.txt backup=yes"
ansible server1 -m command -a "ls /tmp"
ansible server1 -m command -a "ls -l ./tmp/a.txt"
ansible server1 -m file -a "path=/tmp/a.txt owner=tom group=tom mode=0755"
ansible server1 -m command -a "ls -l ./tmp/a.txt"
ansible server1 -m file -a "path=/tmp/a.txt state=absent"
ansible server1 -m file -a "path=/tmp/testdir state=directory"
ansible server1 -m file -a "path=/tmp/abcd.txt state=touch"
ansible server1 -m stat -a "path=a.txt"
ansible server1 -m yum -a "name=httpd state=latest"
ansible server1 -m yum -a "name=httpd state=absent"
```