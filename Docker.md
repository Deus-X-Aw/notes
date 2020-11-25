#### Docker

镜像（image）:

容器（container）:

仓库（repository）:

### Docker的常用命令

**帮助命令**

```shell
docker versions #显示docker的版本信息
docker info #docker的系统信息,包括镜像和容器的数量
docker --help #docker的帮助信息
```

#### 镜像命令

```shell
docker images 显示镜像
$root@123 
REPOSITORY      TAG      IMAGE ID     CREATED    SIZE
#
REPOSITORY	镜像的仓库源
TAG	镜像的标签
IMAGE ID	镜像的id
CREATE	镜像的创建时间
SIZE	镜像的大小

docker images -a #-a all 列出所有的镜像
docker images -q #只显示镜像的id
docker rmi imageid #删除指定的镜像
docker rmi `docker images -a` #删除所有的镜像
```

##### docker search 搜索镜像

```shell
docker search mysql
#可选项，通过条件过滤
--filter=STARS=3000 #搜索出来的镜像要求STARS大于3000
```

##### docker pull 下载镜像

```shell
docker pull mysql(所需要的镜像名)
#等价于
docker pull mysql
docker.io/library/mysql:latest
#指定版本
docker pull mysql:5.7
```

##### docker 删除镜像

```shell
docker rmi -f imageid #删除指定的镜像
docker rmi -f imageid imageid imageid #删除多个镜像
docker rmi -f $(docker images -aq) #删除全部的镜像
```



#### 容器命令

说明：我们有了镜像才可以创建容器，linux,下载一个centos/busybox

```shell
docker pull centos
```

新建容器并启动

```shell
docker run [可选参数] image 目录/bin/bash sh
docker run -d --name imagesid /usr/sbin/init
#参数说明
--name="Name"	#容器的名字 tomcat01 tomcat02 tomcat03 用来区分容器
-d		#以后台的方式运行
-it		#使用交互方式进行，进入容器查看内容
-p		#指定容器的端口 -p 8080：8080
		-p ip:主机端口：容器端口
		-p ip:主机端口：容器端口
		-p 容器端口
		#容器端口
-P		#随机指定端口
```

##### 列出所有的容器

```shell
docker ps #列出当前正在运行的容器
docker ps -a #列出当前正在运行的容器+历史运行过的容器
docker ps -q #只显示容器的编号
docker ps -n=? #显示最近创建的容器
```

##### 退出容器

```shell
exit #退出容器并且停止容器
ctrl + P + Q #容器不停止退出
```

##### 删除容器

```shell
docker kill 容器id #停止容器
docker rm 容器id #删除指定容器
docker rm -f  $(docker ps -aq) #删除所有的容器
docker ps -aq | xargs docker rm #删除所有的容器
```

##### 启动和停止容器的操作

```shell
docker start 容器id #
docker restart 容器id #
docker stop 容器id 
docker kill 容器id 
docker pause 容器id #暂停数据库容器提供服务。
docker unpause 容器id #恢复数据库容器提供服务。
```



### 常用的其他命令

##### 后台启动容器

```shell
#docker run -d 镜像名！
docker run -d centos
#问题docker ps .发现了centos停止了
#常见的问题:docker 容器使用后台运行，就必须要一个前台进程，docker 发现没有应用(或者没有提供服务)，就会自动停止
```

##### 查看日志命令

```shell
docker logs -t -f --tail 容器id#没有容器
自己写一段shell脚本
docker run -d centos /bin/bash -c "while true;do echo Perseus; sleep 1;dones"
#显示日志
-tf	#-f format -t timestamps
--tail number #末尾显示number行
docker logs -tf --tail 10 containerid
```

##### 查看容器中的进程信息ps

```shell
docker top 容器id
UID是用户id 	PID父id是		PPID是进程id
```

##### 查看容器的原数据

```shell
docker inspect 容器id
```

##### 进入当前正在运行的容器

```shell
#容器后台运行
docker exec it 容器id baseshell
docker attach 容器id #正在执行当前的代码

docker exec #进入容器后开启一个新的终端，可以在里面操作（常用）
docker attach #进入容器正在执行的终端，不会重新启动新的进程
```

##### 从容器内拷贝文件

```shell
docker cp 
docker cp 容器id:/路径 主机host的目录
```

#### docker其他的常用的命令

![](../ScreenShotFile/docker0.png)

![](../ScreenShotFile/docker1.png)

##### 安装/运行nginx

```shell
docker search nginx
docker pull nginx
docker run -d --name nginx01 -p 3344:80 nginx
#-d 后台运行
#--name 给容器命名
#-p 宿主机端口：容器内部端口
```

##### 安装/运行Tomcat

```shell
#官方使用
docker run -it --rm tomcat:9.0
#我们之前的启动都是后台，停止了容器之后，容器还是可以查到，  docker run -it --rm,一般用来测试，用完就删除

#下载在启动
docker pull tomcat:9.0
#启动运行
docker run -d -p 3355:8080 --name tomcat01 tomcat
#测试访问没有问题
#进入容器
docker exec -it tomcat01 /bin/bash
#发现问题：1.linux命令少了 2.没有webapps 3.阿里云镜像的原因，默认是最小的镜像，所有的不必要的都剔除了
#保证最小的可运行的环境
```

```shell
docker stats (容器id) #查看cpu的状态
```

```shell
增加内存的限制，修改配置文件 -e 环境配置修改
docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms64m -Xmx512m" elasticsearch:7.6.2
```

##### 可视化

```shell
portainer(先用这个)
docker run -d -p 8080:9000 \
--restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer
```

Rancher(CI/CD再用)



#####  什么是portainer?

docker 图形化界面管理工具，提供一个后台面板供我们操作

```shell
docker run -d -p 8088:9990 \
--restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portaienr/portainer
```

### Docker镜像讲解

##### 镜像是什么

镜像是一种轻量级，可执行的独立软件包，用来打包软件运行环境和基于运行环境开发的软件 ，它包含某个软件所需的所有的内容，包括代码，运行是，库，环境变量和配置文件。

所有有应用，直接打包docker 镜像，就可以运行起来

##### 如何得到镜像：

1。从远程仓库下载

2。朋友拷贝给你

3。自己制作一个镜像DockerFile

#### Docker镜像加载原理

```shell
UnionFS(联合文件系统)
```

我们下载的时候看到的就是这个。

UnionFS( 联合文件系统):Union文件系统(UnionFS )是一种分层、轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下(unite several directories into a single virtualfilesystem)。Union文件系统是Docker镜像的基础。镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像。

特性∶一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录

```shell
Docker镜像加载原理
```

docker 的镜像实际上由一层层的文件系统组成，这种层级的文件系统UnionFS。



bootfs(boot file system)主要包含bootloader和kernel, bootloader主要是引导加载kernel, Linux刚启动时会加载bootfs文件系统，在Docker镜像的最底层是bootfs。这一层与我们典型的Linux/Unix系统是一样的，包含boot加载器和内核。当boot加载完成之后整个内核就都在内存中了，此时内存的使用权已由bootfs转交给内核，此时系统也会卸载bootfs。



rootfs (root file system)，在bootfs之上。包含的就是典型Linux系统中的/dev, /proc, /bin, letc等标准目录和文件。rootfs就是各种不同的操作系统发行版，比如Ubuntu , Centos等等。



对于一个精简的OS , rootfs 可以很小，只需要包含最基本的命令，工具和程序库就可以了，因为底层直接用Host的kernel，自己只需要提供rootfs就可以了。由此可见对于不同的linux发行版, bootfs基本是一致的, rootfs会有差别,因此不同的发行版可以公用bootfs。

##### commit镜像

```shell
docker commit 提交容器成为一个新的副本
#命令和git原理类似
docker commit -m="提交的描述信息" -a="作者" 容器id 目标镜像名：[TAG]
```

#### 容器数据卷

##### 什么是容器数据卷

###### docker的理念回顾

将应用和环境打包成一个镜像。

数据 ？如果数据都在容器中，那么我们如果把容器删除了，数据就会丢失。==需求：数据可以持久化存储==

Mysql，容器删除了，数据库就没了，等于删库跑路。==需求：Mysql的数据可以存储在本地或者是其他的服务器上==

容器之间可以有一个数据共享的技术。Docker容器中产生的数据 ，同步到本地。

这就是卷技术，目录的挂载，将我们容器的内目录，挂载到linux上。

总结一句话:容器的持久化和同步操作。容器之间也是可以进行数据共享的。

#### 使用数据卷

==方式一：直接使用命令来挂载 -v==

```shell
docker run -it -v 主机目录：容器内目录
#测试
docker run -it -v /home/ceshi:/home centos /bin/bash
#启动后可以使用 docker inspect 容器id
#在mount 下面可以看到所挂载的目录
```

好处：我们以后修改只要在本地修改就可以了，容器内会自动同步。

##### 实战：部署mysql

```shell
#思考：mysql是持久化存储问题
docker pull mysql:5.7
#运行容器，需要做数据挂载，做配置，需要配置密码的
docker run -d -p 3310:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:5.7
#启动成功后，我们在本地使用sqlyog来测试下
#sqlyog 连接到服务器的3310 ----- 3310 和容器内的3306 映射，这个时候我们就可以连接上了
```

假设我们把容器删除了，我们挂载到本地的数据卷没有丢失，这就实现了数据的持久化存储功能

#### 具名和匿名挂载

```shell
#匿名挂载
-v 容器内路径
docker run -d -p --name nginx01 -v /etc/nginx nginx
#查看所有有volume的情况
docker volume ls
local  0eafweafjiwefj233123413adjfiwej1234
#这里发现，这种就是匿名挂载，我们在-v只写了容器内的路径，没有写容器外的路径
#具名挂载
docker run -d -p --name nginx02 -v juming-nginx:/etc/nginx nginx
131423412372351aseufiwieqr14awqriq34231kadsfi
docker volume ls
DRIVEER 			VOLUME NAME
local				juming-nginx

#通过-v 卷名：容器内路径
#查看一下路径
docker volume inspect juming-nginx
```

==所有的容器内的卷，没有指定目录的情况下都是在/var/lib/docker/volumes/xxxx/_data==

我们通过具名挂载可以方便的找到我们的一个卷，大多数情况在使用的是`具名挂载`

```shell
#如何确定是具名挂载还是匿名挂载，还是指定路径挂载
-v 容器内路径 #匿名挂载
-v 卷名：容器内路径 #匿名挂载
-v /宿主机路径：容器内路径 #指定路径挂载
```

拓展：

```shell
#通过 -v 容器内路径：ro rw 改变读写权限
ro readonly #只读
rw readwrite #可读可写
#一旦设置了权限，容器对我们挂载出来的内容就有了限定
docker run -d -p --name nginx02 -v juming-nginx:/etc/nginx:ro nginx
docker run -d -p --name nginx03 -v juming-nginx:/etc/nginx:rw nginx
#ro只要看到ro，就说明这个路径只能通过宿主机来操作，容器内部是无法操作的
```



### 初识Dockerfile

Dockerfile就是用来构建docker镜像的构建文件，命令脚本

==方式二==

通过这个脚本可以生成镜像，

```shell
#创建一个dockerfile文件，名字可以随机，但建议用Dockerfile
#文件中的内容指令 （大写） 参数

FROM busybox

VOLUME ["volume01","volume02"]
#属于匿名挂载
CMD echo "----end----"
CMD /bin/bash

#这里的每个命令，就是镜像的一层

#docker build -f /home/docker/docker-test-volume/dockerfile1 -t perseus_busybox:1.0 .

#docker run -it 容器id sh

```

![image-20201120100415077](C:/Users/Deus/AppData/Roaming/Typora/typora-user-images/image-20201120100415077.png)

测试下刚才的文件是否同步出去了

这种方式我们未来用的十分多，因为我们通常会构建自己的镜像

假设构建镜像时没有挂载卷，我们就需要手动挂载，

-v 卷名：容器内的路径



#### 数据卷容器

多个mysql同步数据

```shell
--volumes-from  #用于多个容器之间的挂载
docker run -it --name docker02(子类) --volumes-from docker01(父类) persues_busybox:1.0 sh
```

```shell
#docker01 删除容器，查看docker02，docker03内docker01的内容还存在
```

类似于拷贝，备份



多个mysql实现数据共享

```shell
docker run -d -p 3310:3306 -v /etc/mysql/conf.d -v /var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:5.7
docker run -d -p 3310:3306 -e MYSQL_ROOT_PASSWORD=1233456 --name mysql02 --volumes-from mysql01 mysql:5.7

#这个时候，可以实现两个之容器数据同步
```



##### 结论：

容器之间配置信息的传递，数据卷容器的生命周期一直持续到没有容器使用为止。

但是一旦持久化到本地之后，这个时候，本地的数据是不会删除的。



### DockerFile

dockerfile是用来构建docke镜像的文件，命令参数脚本。

1。编写一个dockerfile文件

2。docker build 构建成为一个镜像

3。docker run 运行一个镜像

4。docker push 发布镜像



#### DockerFile构建过程

基础知识：

1。每个保留关键字（指令）都必须是大写

2。执行从上到下顺序执行

3。#表示注释

4。每一个指令都会创建提交一个新的镜像层，并提交

dockerfile是面向开发的，我们以后要发布项目，做镜像，就需要编写dockerfile文件，这个文件十分简单。

docker镜像逐渐成为企业交付的标准，必须掌握。

DockerFile:构建文件，定义了一切的步骤，源代码

DockerImages:通过DockerFile构建生成的

#### DockerFile的指令

以前我们使用的是别人的镜像，现在我们自己写

```shell
FROM 			#基础镜像，一切从这里开始构建
MAINTAINER 		#镜像是谁写的，姓名+邮箱
RUN 			#镜像构建的时候需要的运行命令
ADD 			#步骤：tomcat镜像，这个tomcat，这个tomcat压缩包。添加内容。
WORKDIR 		#镜像的工作目录
VOLUME 			#挂载的目录 VOLUME ['','']
EXPOST 			#暴露的端口配置
CMD 			#指定这个容器的时候运行的命令，只有最后的一个会生效，可被替代
ENTRYPOINT   	#指定这个容器的时候要运行的命令，可以追加命令
ONBUILD 		#当构建一个被继承DockerFile 这个时候会运行ONBUILD 的指令，触发指令
COPY			#类似ADD，将我们的文件拷贝到镜像中
ENV				#构建的时候设置环境变量
```

![image-20201120162729276](C:/Users/Deus/AppData/Roaming/Typora/typora-user-images/image-20201120162729276.png)

#####  实战测试

Docker Hub 中99%镜像都是从这个基础镜像过来的FROM sratch,然后配置需要的软件和配置来进行构建

==创建自己的centos==

```shell
#1.编写DockerFile的配置文件
[root@190470230 dockerfile]# cat mydockerfile
FROM busybox
MAINTAINER Perseus<3060977800@qq.com>

ENV MYPATH /usr/local
WORKDIR $MYPATH

RUN yum -y install vim
RUN yum -y install net-tools

EXPOSE 80

CMD echo $MYPATH
CMD echo "---end---"
CMD /bin/bash

#2.通过这个文件构建镜像
#命令 docker build -f dockerfile文件路径 -t 镜像名：[tag]
docker build -f mydockerfile-centos -t mycentos:1.0 .

Successfully built 8e31a090c264
Successfully tagged mycentos:1.0

#3.测试运行
```

对比：之前的centos

目录是在/下

我们可以列出本地镜像的变更历史

```shell
docker history 镜像id
```

可以用来查看构建的信息

```shell
CMD和ENTRYPOINT的区别
#编写dockerfile
FROM centos
CMD ["ls","-a"]

ENTRYPOINT ["ls","-a"]

CMD				#docker run 容器id -l会报错（目的是为了显示 ls -al，不可追加命令） 只能写原生命令
ENTYPOINT		#docker run 容器id -l不会报错（目的是为了显示 ls -al，可以再后面追加命令）
```

DockerFile中很多的命令都是十分相似的，我们需要小心区分它

#### 实战：Tomcat镜像

1.准备镜像文件tomcat压缩包，jdk的压缩包

2.编写docker文件，官方命命名==Dockerfile==，build会自动寻找这个文件，就不需要-f指定了

```shell
FROM centos
MAINTAINER kuangshen<24736743@qq.com>
COPY readme.txt /usr/local/ readme .txt
ADD jdk-8u11-linux-x64.tar.gz /usr/local/ADD apache-tomcat-9.0.22.tar.gz /usr/local/
RUN yum -y install vim
ENV MYPATH /usr/localWORKDIR $MYPATH
ENV JAVA_HOME /usr/local/jdk1.8.0_11
ENV CLASSPATH $JAVA_HOME/iib/dt.jar: $JAVA_HOME/lib/tools.jarENV CATALINA_HOME/usr/local/apache-tomcat-9.0.22
ENV CATALINA_BASH /usr/local/apache-tomcat-9.0.22
ENV PATH $PATH:$JAVA_HOME/bin :$CATALINA_HOME/lib:$CATALINA_HOME/bin
EXPOSE 8080
CMD /usr/local/apache-tomcat-9.0.22/bin/startup.sh  tail -F /url/local/apache-tomcat-9.0.22/bin/logs/catalina.out
```

3.构建镜像

```shell
#docker build -t diytomcat .
```

4.启动镜像

5.访问测试

6.发布项目（由于做了挂载卷，我们直接在本地编写项目就可以发布了！）

![image-20201120203334954](C:/Users/Deus/AppData/Roaming/Typora/typora-user-images/image-20201120203334954.png)

![image-20201120203407007](C:/Users/Deus/AppData/Roaming/Typora/typora-user-images/image-20201120203407007.png)

##### 发布自己的镜像

==DockerHub==

```shell
注册DockerHub，并登陆
在我们的服务器上提交
docker login --help
docker login 
-p password
-u username

docker login -u username -p Password

docker push name/images:id

#如果报错，改标签
docker tag imageid name
docker push name:/images:id
#自己发布，记得写好标签，版本号
#提交的时候也是按照镜像的层级来进行提交的
```

==阿里云==

```shell

```



### Docker网络

##### 理解网络

Docker0

相当于路由器

桥接网络

```shell
	#我们发现这个容器带来网卡，都是一对对的
	#evth-pair 就是一对的虚拟设备接口，他们都是成对出现的，一段连着协议，一段彼此相
	#正因为有这个特性，evth-pair充当一个桥梁，连接各种虚拟网络设备的
	# openstack，Docker容器之间的连接，OVs的连接，都是使用evth-pair技术
```

```shell
#如何可以解决呢?
#通过--7ink既可以解决了网络连通问题
# docker run -d -P --name tomcat03 --1ink tomcat02 					tomcat5ca72d80ebb048d3560df1400af03130f37ece244be2a54884336aace2106884
# docker exec -it tomcat03 ping tomcat02 PING tomcat02
(172.18.0.3)56(84) bytes of data.
64 bytes from tomcat02 (172.18.0.3): icmp_seq=1 ttl=64 				time=0.100 ms64 bytes from tomcat02 (172.18.0.3): icmp_seq=2 ttl=64 time=0.066 ms64 bytes from tomcat02 (172.18.0.3): icmp_seq=3 tt1=64 time=0.067 ms
#反向可以ping通吗?
# docker exec -it tomcat02 ping tomcat03ping: tomcat03 : Name or service not known

```

![image-20201121174842238](C:/Users/Deus/AppData/Roaming/Typora/typora-user-images/image-20201121174842238.png)

本质探究:--link就是我们在hosts配置中增加了一个172.18.0.3 tomcat02 312857784cd4我们现在玩Docker已经不建议使用--link了 !
自定义网络!不适用docker0 !
docker0问题:他不支持容器名连接访问!

#### 网络模式

```shell
显示网络
docker network ls
```

```shell
#docker 默认是--net bridge 这就是我们的docker0
docker run -d -P --name tomcat01 tomcat
docker run -d -P --name tomcat01 --net bridge tomcat

#docker0特点:默认，域名不能访间， --link可以打通连接!

#我们可以自定义一个网络!
[rootokuangshen /]# docker network create --driver bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1 mynet
eb21272b3a35ceaba11b4aa5bbff131c3fb09c4790f0852ed4540707438db052
[root@kuangshen /]# docker network 1s
NETWORK ID
NAME
DRIVER
sCOPE
5a008c015cac
bridge
bridge
local
db44649a9bff
composetest_default bri dge
local
ae2b6209c2ab
host
host
local
eb21272b3a35
mynet
bridge
local
```





bridge:桥接docker(默认，自己床架也使用bridge模式)none : 不配置网络
host:和宿主机共享网络
container:容器网络连通!(用的少!局限很大)

#### 测试

![image-20201121181409173](C:/Users/Deus/AppData/Roaming/Typora/typora-user-images/image-20201121181409173.png)

```shell
docker network connect mynet 容器id(容器名)
```



![image-20201121192049628](C:/Users/Deus/AppData/Roaming/Typora/typora-user-images/image-20201121192049628.png)

![image-20201121192756505](C:/Users/Deus/AppData/Roaming/Typora/typora-user-images/image-20201121192756505.png)

## Docker Compose

Docker compose来轻松高效的管理容器.定义运行多个容器

1.Dockerfile保证我们的项目在任何地方可以运行

2.service

​	docker-compose.yml

3.启动项目



作用:批量容器编排

```shell
我的理解
```

compose是docker官方的开源项目.需要安装

dockerfile 让程序在任何地方运行.

 compose:重要的概念

​	`服务services,容器,应用,(web,redis,MySQL....)`

​	`项目project,一组关联的容器,博客,web,mysql,wp`



#### docker compose下载

```shell
#国内 http://get.daocloud.io/ 下载 

# curl -L https://get.daocloud.io/docker/compose/releases/download/1.25.4/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
#授权
# chmod +x /usr/local/bin/docker-compose
```

验证

docker-compose --help

docker-compose --version

##### docker yaml规则(yml)

```shell
#3层
version: '' #版本
service: #服务 
  服务1:web
    #服务配置
    images
    build
    network
    ....
   服务2:redis
    ....
   服务3:
#其他配置 网络/卷,全局规则
volumes:
networks:
configs:
```



#### 开源项目:(启动)

#### 博客

下载程序,安装数据库,配置.....

compose应用.=> 一键启动

1.下载项目(docker-compose.yml)

2.如果需要文件.dockerfile

3.文件准备ok(直接启动)



前(后)台启动

docker -d

docker-compose up -d

so easy !



linux,docker,k8s

12-20k

