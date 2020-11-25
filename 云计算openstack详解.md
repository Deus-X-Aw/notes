# Iaas

**任务一、IaaS 云平台搭建 基础环境：**

1.使用命令行方式设置主机名，防火墙以及 SELinux 设置如下：

（1）设置控制节点主机名 controller；计算节点主机名：compute。

**hostnamectl set-hostname controller**

**hostnamectl set-hostname compute**

（2）各个节点关闭防火墙，设置开机不启动。

**systemctl stop firewalld**

**systemctl  disable firewalld**

（3）设置各个节点 selinux 状态为 permissive。

**vi /etc/selinux/conf**

**SELINUX=permissive**

**setenforce 0**

2.使用命令查询控制/计算节点的主机名。

**hostname**

3.使用命令查询控制/计算节点 selinux 的状态。

**getenforce**

4.在控制节点上通过 SecureFX 上传两个镜像文件 CentOS-7-x86_64-DVD-1511.iso， XianDian-IaaS-v2.2.iso 到 opt 下，使用命 令创建/opt 下两个目录，并将以上镜像文件分别挂载到上述两个目录下，并 使用命令查看挂载的情况（需显示挂载的文件系统类型和具体的大小）。

5.在控制节点上通过 SecureFX 上传两个镜像文件 CentOS-7-x86_64-DVD-1511.iso， XianDian-IaaS-v2.2.iso 到 opt 下，通过命 令行创建两个目录，并将以上镜像文件分别挂载到上述两个目录下。

6.配置控制节点本地 yum 源文件 local.repo ，搭建 ftp 服务器指向存放 yum 源路径；配置计算节点 yum 源文件 ftp.repo 使用之前配置的控制节点 ftp 作 为 yum 源，其中的两个节点的地址使用主机名表示。使用 cat 命令查看上述 控制/计算节点的 yum 源全路径配置文件。

**controller:**

**[centos]**

**name=centos**

**baseurl=file:///opt/centos**

**enabled=1**

**gpgcheck=0**

**[iaas]**

**name=iaas**

**baseurl=file:///opt/iaas/iaas-repo**

**enabled=1**

**gpgcheck=0**

**compute:**

**[centos]**

**name=centos**

**baseurl=ftp://controller/centos**

**enabled=1**

**gpgcheck=0**

**[iaas]**

**name=iaas**

**baseurl=ftp://controller/iaas/iaas-repo**

**enabled=1**

**gpgcheck=0**

7.在控制节点和计算节点分别安装 iaas-xiandian 软件包，完成配置文件中基 本变量的配置，并根据提供的参数完成指定变量的配置。

yum  install iaas-xiandian   -y

**sed  -i  -e  "s/PASS=/PASS=000000/"  -e "s/^#//"  /etc/xiandian/openrc.sh**

**vi /etc/xiandian/openrc.sh**

**HOST_IP=192.168.100.40**

**HOST_NAME=controller**

**HOST_IP_NODE=192.168.100.50**

**HOST_NAME_NODE=compute**

**RABBIT_USER=openstack**

**RABBIT_PASS=000000**

**DB_PASS=000000**

**DOMAIN_NAME=demo**

**ADMIN_PASS=000000**

**DEMO_PASS=000000**

**KEYSTONE_DBPASS=000000**

**GLANCE_DBPASS=000000**

**GLANCE_PASS=000000**

**NOVA_DBPASS=000000**

**NOVA_PASS=000000**

**NEUTRON_DBPASS=000000**

**NEUTRON_PASS=000000**

**METADATA_SECRET=000000**

**INTERFACE_NAME=eno2**

**minvlan=101**

**maxvlan=200**

**CINDER_DBPASS=000000**

**CINDER_PASS=000000**

**BLOCK_DISK=sda3**

**TROVE_DBPASS=000000**

**TROVE_PASS=000000**

**SWIFT_PASS=000000**

**OBJECT_DISK=sda4**

**STORAGE_LOCAL_NET_IP=192.168.100.50**

**HEAT_DBPASS=000000**

**HEAT_PASS=000000**

**CEILOMETER_DBPASS=000000**

**CEILOMETER_PASS=000000**

**AODH_DBPASS=000000**

**AODH_PASS=000000**

**Mysql 搭建：**

1.根据平台安装步骤安装至数据库服务，使用一条命令安装提供的 iaas-install-mysql.sh 脚本并查看脚本运行的时间。

**time  iaas-install-mysql.sh**

2.使用 root 用户登录数据库，查询数据库列表信息。

**mysql -uroot -p000000**

**show databases;**

3.使用 root 用户登录数据库，使用 mysql 数据库，查询所有表的信息。

**mysql -uroot -p000000**

**use  mysql**

**show tables;**

4.使用 root 用户登录数据库，使用 mysql 数据库，查询所有表的信息，并查 询表 user 中的host,user,password 特定的信息。

**mysql -uroot -p000000**

**use  mysql**

**show tables;**

**select host,user,password  from  user;**

**Keystone 搭建：**

1.按要求安装完 keystone 脚本后，在数据库中查询 keystone 用户的远程访问 权限信息。

**mysql -uroot -p000000**

**show grants for "keystone"@"%";**

2.列出数据库 keystone 中的所有表。

**mysql  -uroot  -p000000**

**use  keystone**

**show tables;**

3.使用相关命令，查询角色列表信息。

**openstack role list**  

4.使用相关命令，查询 admin 项目信息。

**openstack project show admin**

5.使用相关命令，查询用户列表信息。

**openstack user list**

6.使用相关命令，查询 admin 用户详细信息。

**openstack user show admin**

7.使用相关命令，查询服务列表信息。

**openstack server list**

8.使用一条命令将 keystone 的数据库导出为当前路径下的 keystone.sql 文件， 并使用命令查询文件 keystone.sql 的大小。

**mysqldump -uroot -p000000  keystone > keystone.sql**

**ls -lh  keystone.sql**

**Glance 搭建：**

1.根据平台安装步骤安装至镜像服务，在控制节点使用提供的脚本 iaas-install-glance.sh 安装 glance 组件。使用镜像文件 CentOS_7.2_x86_64_XD.qcow2 创建 glance 镜像名为 CentOS7.2，格式为 qcow2。

**glance image-create  --name "centos7"  --disk-format "qcow2" --container-format  bare --progress < /root/CentOS_7.2_x86_64_XD.qcow2**

2.使用相关命令查询镜像列表，并查询 CentOS7.2 镜像的详细信息。

**glance image-list**

**glance image-show 760be10f-8738-408d-a6dd-abad15fd6930**

3.使用相关命令，在一条命令中查询 glance 组件中所有服务的状态信息。

**systemctl status openstack-glance \*nn**

**Nova 搭建：**

1.根据平台安装步骤安装至 nova 计算服务，在控制节点使用提供的脚本 iaas-install-nova-controller.sh、在计算节点使用提供的脚本 iaas-install-nova-compute.sh，安装 nova 组件。

**controller:**

**iaas-install-nova-controller.sh**

**compute:**

**iaas-install-nova-compute.sh**

2.使用相关命令查询计算节点虚拟机监控器的状态。

**nova hypervisor-stats**

3.使用相关命令查询 nova 服务状态列表。

**nova service-list**

4.使用相关命令查询网络的列表信息。

**nova network-list**

5.使用相关命令查询 nova 资源使用情况的信息。

**nova usage-list**

**Neutron 搭建：**

1.根据平台安装步骤安装至 neutron 网络服务，在控制节点和计算节点通过 提供的 neutron 脚本，完成 neutron 服务在控制节点和计算节点的安装，并配 置为 GRE 网络。



2.根据平台安装步骤安装至 neutron 网络服务，在控制节点和计算节点通过 提供的 neutron 脚本，完成 neutron 服务在控制节点和计算节点的安装，并配置为 VLAN 网络。

3.使用相关命令查询网络服务的列表信息，并以下图的形式打印出来。

![img](http://note.youdao.com/yws/public/resource/5512671a1e02c14d618d6b380b5c288f/xmlnote/5845B012E4EF4AE8815DD161C000D1A8/4901)

**neutron agent-list -c binary -c agent_type -c alive**

4.使用相关命令查询网络服务的列表信息中的“binary”一列。

**neutron agent-list  -c  binary**

5.使用相关命令查询网络服务 DHCP agent 的详细信息。

**neutron agent-list**

+--------------------------------------+--------------------+------------+-------------------+-------+----------------+---------------------------+

| id                  | agent_type     | host    | availability_zone | alive | admin_state_up | binary           |

+--------------------------------------+--------------------+------------+-------------------+-------+----------------+---------------------------+

| 29146532-5ee5-4885-839a-4509968d3e52 | L3 agent      | controller | nova        | :-)  | True      | neutron-l3-agent      |

| 8a05e29f-f217-47de-8c40-7a716cd16f31 | Metadata agent   | controller |          | :-)  | True      | neutron-metadata-agent   |

| 9ca03127-4108-466a-a398-aef386bfe37d | Loadbalancer agent | controller |          | :-)  | True      | neutron-lbaas-agent    |

| a590b488-dfa2-4b5b-b46a-beefa4510dac | Metadata agent   | compute   |          | :-)  | True      | neutron-metadata-agent   |

| bf902fcf-aad7-4f76-aef4-d8cf79fabae8 | DHCP agent     | controller | nova        | :-)  | True      | neutron-dhcp-agent     |

| ce51aac9-166b-42d1-8130-f68b13be5e2f | Open vSwitch agent | controller |          | :-)  | True      | neutron-openvswitch-agent |

| da1a3947-7ee3-452f-ae2e-fcbb194c675d | Open vSwitch agent | compute   |          | :-)  | True      | neutron-openvswitch-agent |

+--------------------------------------+--------------------+------------+-------------------+-------+----------------+---------------------------+

**neutron agent-show bf902fcf-aad7-4f76-aef4-d8cf79fabae8**

6.使用 ovs-vswitchd 管理工具的相关命令查询计算节点的网桥列表信息。

**ovs-vsctl list-br**

7.使用 ovs-vswitchd 管理工具的相关命令查询控制节点的网桥 br-ex 的端口 列表信息。

**ovs-vsctl list-ports br-ex**

8.创建云主机外部网络 ext-net，子网为 ext-subnet，云主机浮动 IP 可用网段 为 192.168.200.100 ~ 192.168.200.200，网关为 192.168.200.1。

**neutron net-create ext-net  --router:external**

**neutron subnet-create  ext-net  --name ext-subnet  --allocation-pool start=192.168.200.10,end=192.168.200.200 --disable-dhcp --gateway 192.168.200.1  192.168.200.0/24**

创建云主机内 部网络 int-net1，子网为 int-subnet1，云主机子网 IP 可用网段为 10.0.0.100 ~ 10.0.0.200，网关为10.0.0.1；

**neutron net-create  int-net1**

**neutron subnet-create  int-net1  --name int-subnet1  --allocation-pool start=10.0.0.100,end=10.0.0.200  --enable-dhcp --gateway 10.0.0.1  10.0.0.0/24**

创建云主机内部网络int-net2，子网为 int-subnet2， 云主机子网 IP 可用网段为 10.0.1.100 ~ 10.0.1.200，网关为 10.0.1.1。

**neutron net-create int-net2**  

**neutron  subnet-create  int-net2  --name int-subnet2  --allocation-pool start=10.0.1.100,end=10.0.1.200 --enable-dhcp  --gateway 10.0.1.1  10.0.1.0/24**

添加名 为 ext-router 的路由器，添加网关在 ext-net 网络，添加内部端口到 int-net1 网络，完成内部网络 int-net1 和外部网络的连通。

**neutron router-create  ext-router**

**neutron  router-gateway-set ext-router  ext-net**

**neutron  router-interface-add  ext-router  int-subnet1**

9.使用相关命令查询所创建路由器的详细信息。

**neutron router-show ext-router**

10.使用相关命令查询所创建子网的列表信息，并查看内网子网的详细信息。

**neutron subnet-list**

+--------------------------------------+-------------+------------------+------------------------------------------------------+

| id                  | name     | cidr       | allocation_pools                   |

+--------------------------------------+-------------+------------------+------------------------------------------------------+

| cb71a8f3-01cd-4c6e-a15a-b77a4ad44dd3 | ext-subnet  | 192.168.200.0/24 | {"start": "192.168.200.2", "end": "192.168.200.254"} |

| df801f44-9f4a-4481-9424-081326474069 | int-subnet1 | 10.0.0.0/24    | {"start": "10.0.0.2", "end": "10.0.0.254"}      |

| 46f50de0-991e-4e25-a0d3-7eff9ef719cf | int-subnet2 | 10.0.1.0/24    | {"start": "10.0.1.2", "end": "10.0.1.254"}      |

+--------------------------------------+-------------+------------------+------------------------------------------------------+

**neutron subnet-show  df801f44-9f4a-4481-9424-081326474069**

**neutron subnet-show  46f50de0-991e-4e25-a0d3-7eff9ef719cf**

11.使用相关命令查询所创建网络的列表信息。

**Dashboard 搭建：**

1.通过脚本 iaas-install-dashboard.sh 安装 dashboard 组件，使用 curl 命令查询网址 http://192.168.100.10/dashboard。

**curl -v http://192.168.100.40/dashboard/**

2.通过脚本 iaas-install-dashboard.sh 安装 dashboard 组件，通过 chrome 浏览 器使用 admin 账号登录云平台网页，进入管理员菜单中的系统信息页面。

**Heat 搭建：**

1.在控制节点使用提供的脚本 iaas-install-heat.sh 安装 heat 组件。

2.使用 heat 相关命令，查询 stack 列表。

**heat stack-list**

3.从考试系统附件下载 server.yml 文件，通过命令行使用 server.yml 文件创 建栈 mystack，指定配置参数为镜像 CentOS7.2、网络 int-net2。

4.查询栈 mystack 状态为 CREATE_COMPLETE 的事件详细信息。

**openstack stack create --template server.yaml --parameter "image=CentOS7.2;network=int-net2" mystack**

**heat stack-create --template-file server.yml --parameters "image=centos7;network=int-net2" mystack**

5.查询栈 mystack 的事件列表信息。

**heat event-list  mystack**

**Trove 搭建：**

1.在控制节点配置提供的脚本 iaas-install-trove.sh，使其连接的网络为 int-net1， 安装数据库 trove 服务，完成 trove 服务在控制节点的安装。完成后查询所有 的数据库实例列表。

**vi /usr/local/bin/iaas-install-trove.sh**

**crudini  --set /etc/xtrove/trove.conf DEFAULT default_neutron_networks c171fe62-ba6a-4077-9da6-495f817462a2**

2.在控制节点上传提供的 MySQL_5.6_XD.qcow2 到系统内，并创建 mysql 的数据库存储类型，使用上传的镜像更新该数据库类型的版本信息和镜像信息。

glance image-create --name mysql5.6  --disk-format "qcow2"  --container-format  bare  --progress < /root/ MySQL_5.6_XD.qcow2

trove-mange datastore_update mysql ""

trove-manage datastore_version_update mysql5.6 mysql

3.在控制节点查创建名称为 mysql-1，大小为 5G，数据库名称为 myDB、远 程连接用户为 user，密码为 r00tme，类型为 m1.small 完成后查询 trove 列表 信息，并查询 mysql-1 的详细信息。

**trove create mysql-1 m1.small --size 5 --databases myDB --user  root:000000 --datastore mysql --datastore_version 5.6**

**trove list**

**trove show mysql-1**

4.在控制节点查询所有数据的版本信息，完成后查询 mysql 数据库的详细信息。

**trove databastore-version-list mysql**

**trove datastore-show mysql**

**任务二、IaaS 云平台运维**

**Rabbitmq 运维：**

1.按以下配置在云平台中创建云主机，完成本任务下的相关试题后关闭云主机。

云主机： （1）名称：IaaS

（2）镜像文件：Xiandian-IaaS-All.qcow2

（3）云主机类型：4cpu、8G 内存、100G 硬盘

（4）网络：网络 1：int-net1，绑定浮动 IP 网络 2：int-net2 注：该镜像已安装 IaaS 平台所有可能使用的组件，用于完成 IaaS 平台相关 运维操作题，必须按以上配置信息配置接入两个网络才能保证云主机运行正常。 根据题目要求，连接相应的云主机或各节点服务器，进行以下答题。

2.使用 rabbitmqctl 创建用户 xiandian-admin，密码为 admin。

**rabbitmqctl add_user xiandian-admin adminra**

3.使用 rabbitmqctl 命令查询所有用户列表。

**rabbitmqctl list_users**

4.使用命令对 xiandian-admin 用户进行授权，对本机所有资源可写可读权限。

**rabbitmqctl set_permissions  xiandian-admin ".\*" ".\*" ".\*"**

5.使用 rabbitmqctl 命令查询集群状态。

**rabbitmqctl cluster_status**

6.使用命令给 xiandian-admin 用户创建 administrator 角色，并查询。

**rabbitmqctl set_user_tags xiandian-admin administrator**

**rabbitmqctl list_users**

7.使用命令对用户 xiandian-admin 进行授权，对本机所有资源可写可读权限， 然后查询 xiandian-admin 用户的授权信息。

**rabbitmqctl list_user_permissions xiandian-admin**

8.使用 rabbitmqctl 命令，查看队列信息，所包含的信息包括 name，arguments， messages，memory。

**rabbitmqctl list_queues name arguments messages memory**

9.通过修改配置文件的方式修改 memcache 的缓存大小，使用 ps 相关命令查询 memcahce 进程的信息，将修改的配置文件全路径文件名、修改的参数以 及相应的参数值、查询 memcache 进程信息。

**vi /etc/syscoofig/memcached**

**CACHESIZE="256"**

**systemctl restart memcached.service**

**[root@controller ~]# ps -aux | grep memcached**

**memcach+ 41013  0.2  0.0 325640  1248 ?     Ssl  15:05  0:00 /usr/bin/memcached -u memcached -p 11211 -m 256 -c 1024**

**root   41036  0.0  0.0 112644  952 pts/0   S+  15:05  0:00 grep --color=auto memcached**

10.构建 rabbitmq 集群，并对集群进行运维操作。

**Mysql 运维：**

**mysql -uroot  -p000000**

1.使用数据库的相关命令查询数据库的编码方式。

**show variables like 'character_set_database';**

2.通过 mysql 相关命令查询当前系统时间。

**select now();**

**select sysdate();**

3.通过 mysql 相关命令，查看当前是什么用户。

**select user();**

4.通过 mysql 相关命令，查看 mysql 的默认存储引擎信息，并查看 mysql 支 持的存储引擎有哪些。

**show variables like '%storage_engine%';**

**show engines;**

5.进入数据库 keystone，通过 user 表和 local_user 表做联合更新，u 用来做 user 表别名，lu 用来做 local_user 表别名，sql 语句更新 neutron 用户的 enabled 状态为 0， 更新语句中 user 表在 local_user 前面。

**update user as u join local_user as lu on u.id-lu.user_id set enabled=0  where lu.name='neutron';**

6.进入数据库 keystone，通过 user 表和 local_user 表做联合查询，u 用来做 user 表别名，lu 用来做 local_user 表别名，两表联合查询 nova 用户的 enabled 状态，查询语句中 user 表在 local_user 前面。

**select enabled from user u join local_user lu on u.id=lu.user_id where lu.name='nova';**

7.进入数据库，创建本地用户 examuser，密码为 000000，然后查询 mysql 数据库中的 user 表的特定字段。最后赋予这个用户所有数据库的“查询”“删 除”“更新”“创建”的本地权限。

**create user 'testuser'@'localhost' identified by '000000';**

**select  host，user，password from user;**

**grant select,delete,update,create on \*.\* to examuser@'localhost' identified by '000000';**

8.登录 iaas 云主机，登录 mysql 数据库，使用 keystone 数据库，查询本地用户表中的所有信息，并按照 id 的降序排序。（关于数据库的命令均使用小写）

**mysql -uroot  -p000000**

**use keystone;**

**select \* from user order by id desc;**

**MongoDB 运维**

1.登录 iaas 云主机，登录 MongoDB 数据库，查看数据库，使用 ceilometer 数据库，查看此数据库下的集合，并查询此数据库用户，最后创建一个数据库并查询。

**mongo**

**show dbs;**

**use ceilometer**

**show collections;**

**show users;**

**use  test**

**show  dbs**  （新建数据库要向里面插入数据之后才会在列表中显示）

2.登录 iaas 云主机，登录 MongoDB 数据库，新建一个数据库，使用这个数据库，向集合中插入数据，最后查询特定的一类数据。

**use new-db**

**db.new_collection.insert({"name":"zhangsan","age":21})**

**db.new_collection.insert({"name":"lisi","age":"22"})**

3.登录 iaas 云主机，登录 MongoDB 数据库，新建一个数据库，使用这个数据库，向集合中插入数据，插入完毕后，数据进行修改，修改完后，查询修改完的数据。

**use student**

**db.student.insert({"name":"xiaoming"});**

**db.student.update({"name":"xiaoming"},{"name":"wenwen"});**

**db.student.find()**

4.登录 iaas 云主机，登录 MongoDB 数据库，新建一个数据库，使用这个数据库，向集合中插入数据（其中某一条数据插入两遍），插入数据完毕后，发现某条数据多插入了一遍需要删除，请使用命令删除多余的一行数据，最后将数据库删除。

**> use class**

switched to db class

**> db.class.insert({"name":"1"},{"class":"1"});**

WriteResult({ "nInserted" : 1 })

\>

**> db.class.insert({"name":"1"},{"class":"1"});**

WriteResult({ "nInserted" : 1 })

**> db.class.insert({"name":"2"},{"class":"2"});**

WriteResult({ "nInserted" : 1 })

\>

**> db.class.find()**

{ "_id" : ObjectId("5daf83f4b7870f79e5a4baf1"), "name" : "1" }

{ "_id" : ObjectId("5daf83fab7870f79e5a4baf2"), "name" : "1" }

{ "_id" : ObjectId("5daf8406b7870f79e5a4baf3"), "name" : "2" }

**> db.class.remove({"_id":"ObjectId("5daf83fab7870f79e5a4baf2")"})**

5.登录 iaas 云主机，登录MongoDB 数据库，新建一个数据库，使用这个数据库，向集合中插入数据，插入完毕后，查询集合中的数据并按照某关键字的升序排序。

6.登录 iaas 云主机，登录MongoDB 数据库，新建一个数据库，使用这个数据库，向集合中批量插入多条数据，使用 for 循环，定义变量 i=1，插入"_id" : i，"name" : "xiaoming"， "age" : "21"。插入数据完毕后，统计集合中的数据条数，然后查询集合中满足特定条件的结果。

7.登录 iaas 云主机，使用 mongoimport 命令，将给定的文件，导入至 MongoDB 下的相应数据库中的指定集合中。导入后登录 MongoDB 数据库。查询集合 中满足特定条件的结果。注：PPG--场均得分；PTS--总得分；FG%--投篮命 中率；3P%--三分命中率；MPG--平均上场时间

**Keystone 运维：**

1.在 keystone 中创建用户 testuser，密码为 password。

**openstack user create --domain demo --password password testuser**

2.将 testuser 用户分配给 admin 项目，赋予用户 user 的权限。

**openstack role add --project admin --user testuser user**

3.以管理员身份将 testuser 用户的密码修改为 000000。

**openstack user set --password 000000 testuser**

4.通过 openstack 相关命令获取 token 值。

**openstack token issue**

5.使用命令查询认证服务的查询端点信息。

**openstack endpoint list | grep keystone**

6.使用命令列出认证服务目录。

**openstack catalog list**

7.在 keystone 中创建用户 testuser，密码为 password，创建好之后，使用命令修改 testuser 密码为 000000，并+查看 testuser 的详细信息。

**openstack user create --domain demo --password password testuser**

**openstack user set --password 000000  testuser**

**openstack user show testuser**

8.在 keystone 中创建用户 testuser，密码为 password，创建好之后，使用命令修改 testuser 的状态为 down，并查看 testuser 的详细信息。

**openstack user set --disable testuser**

**openstack user show testuser**

9.完成 keystone 证书加密的 HTTPS 服务提升。



**Glance 运维：**

1.使用 glance 相关命令上传 CentOS_6.5_x86_64_XD.qcow2 镜像到云主机中， 镜像名为 testone，然后使用 openstack 相关命令，并查看镜像的详细信息。

**glance image-create --name "testone" --disk-format "qcow2"  --container-format bare --progress < /root/CentOS_6.5_x86_64_XD.qcow2**

**openstack image show testone**

2.使用glance相关命令上传两个镜像，一个名字为testone，一个名字叫testtwo， 使用相同的镜像源 CentOS_6.5_x86_64_XD.qcow2，然后使用 openstack 命令 查看镜像列表，接着检查这两个镜像的 checksum 值是否相同。

**glance image-create --name "testone" --disk-format "qcow2"  --container-format bare --progress < /root/CentOS_6.5_x86_64_XD.qcow2**

**glance image-create --name "testtwo" --disk-format "qcow2"  --container-format bare --progress < /root/CentOS_6.5_x86_64_XD.qcow2**

**openstack image list**

**openstack image show testone | grep checksum**

**openstack image show testtwo | grep checksum**

3.登录 iaas 云主机，使用 glance 相关命令，上传镜像，源使用 CentOS_6.5_x86_64_XD.qcow2，名字为 testone，然后使用 openstack 命令修 改这个镜像名改为 examimage，改完后使用 openstack 命令查看镜像列表。

**glance image-create --name "testone" --disk-format "qcow2"  --container-format bare --progress < /root/CentOS_6.5_x86_64_XD.qcow2**

**openstack image set testone --name "examimage"**

**openstack image list**

4 使用 glance 相关命令，上传镜像，源使用 CentOS_6.5_x86_64_XD.qcow2， 名字为 examimage，然后使用 openstack 命令查看镜像列表，然后给这个镜 像打一个标签，标签名字为 lastone，接着查询修改的结果。

**glance image-create --name "examimage" --disk-format "qcow2"  --container-format bare --progress < /root/CentOS_6.5_x86_64_XD.qcow2**

**openstack image list**

**openstack image set examimage --tag lastone**

**openstack image show  examimage**

5.通过一条组合命令获取镜像列表信息，该组合命令包含：

（1）使用 curl 命令获取镜像列表信息；

（2）使用 openstack 相关命令获取的 token 值；

（3）仅使用 awk 命令且用“|”作为分隔符获取 token 具体参数值。

**curl -i -H "X-Auth-Token:$(openstack token issue | awk -F "|" '/\sid\s/{print $3}')" http://controller:9292/v2/images**

6.通过一条组合命令获取该镜像详细信息，该组合命令要求：

（1）不能使用任何 ID 作为参数；

（2）使用 openstack 相关命令获取详细信息；

（3）使用 glance 相关命令获取镜像对应关系；

（4）仅使用 awk 命令且用“|”作为分隔符获取 ID 值。

**openstack image show `glance image-list | awk -F "|" '/examimage/{print $2}'`**

7.查看 glance 配置文件，找到默认的镜像存储目录，进入到存储目录中，使用 qemu 命令查看任意的一个镜像信息。

**cat /etc/glance/glance-api.conf | grep filesystem_store_datadir**

**cd /var/lib/glance/images/**

**qemu-img info 05cf4c95-cf94-44a6-8ccd-ff9969dc39e2**

**Nova 运维：**

1.修改云平台中默认每个 tenant 的实例注入文件配额大小，并修改。

**nova quota-class-update default --injected-file-content-bytes 102400**

**nova quota-defaults**

2.通过 nova 的相关命令创建云主机类型,名字exam，ID为1234，内存为1024，硬盘为20G，虚拟内核数量为2，并查询该云主机的详细信息。

**nova flavor-create exam 1234 1024 20 2**

**nova flavor-show exam**

3.使用 nova 相关命令，查询 nova 所有服务状态。

**nova service-list**

4.修改云平台中默认每个 tenant 的实例配额个数并查询。

**nova quota-class-update default --instances 20**

**nova quota-defaults**

5.使用 nova 相关命令，查询 nova 所有的监控列表，并查看监控主机的详细信息。

**nova hypervisor-list**

**nova hypervisor-show **   hypervisor为参数  可通过  nova hypervisor-list  获得

6.使用 grep 命令配合-v 参数控制节点/etc/nova/nova.conf 文件中有效的命令行覆盖输出到/etc/novaback.conf 文件。

**grep -v ^# /etc/nova/nova.conf | grep -v ^$ > /etc/novaback.conf**

7.此题可使用物理 iaas 环境，使用 nova 相关命令，启动一个云主机，云主 机类型使用 m1.small，镜像使用 CentOS_6.5_x86_64_XD.qcow2，云主机名 称为 examtest。

**nova boot --flavor m1.small --image CentOS_6.5_x86_64_XD.qcow2 --nic net-id=c5221f87-5f27-4fb0-9e31-08269c104676 examtest**

8.此题可使用物理 iaas 环境，使用 openstack 相关命令，启动一个云主机， 云主机类型使用 m1.small，镜像使用 centos6.5，云主机名称为 xxxtest，并 使用 openstack 命令查看此云主机的详细信息。

**openstack server create --flavor m1.small --image examimage --nic net-id=c5221f87-5f27-4fb0-9e31-08269c104676 xxxtest**

**openstack server show xxxtest**

9.此题可使用物理环境，登录 dashboard 界面，创建一台虚拟机，将该虚拟机使用手动迁移的方式，迁移至另一个计算节点并查看。（controller 既是 控制也是计算）



10.登录 iaas-all 云主机，修改 nova 后端默认存储位置。

11.修改相应的配置文件，使得 openstack 云主机的工作负载实现所要求的性 能、可靠性和安全性。

12.配置 NFS 网络存储作为 nova 的后端存储。

**Cinder 运维：**

1.使用分区工具，对/dev/vda 进行分区，创建一个分区，使用命令将刚创建的分区创建为物理卷，然后使用命令查看物理卷信息。

**fdisk /dev/vda**

**pvcreate /dev/vda1**

**pvdisplay**

2.使用命令查看当前卷组信息，使用命令创建逻辑卷，查询该逻辑卷详细信息。

**lvcreate -L 20G  -n lv1 vg1**

**vgdisplay**

3.创建一个卷类型，然后创建一块带这个卷类型标识的云硬盘，查询该云硬盘的详细信息。

**openstack volume type create type1**

**openstack volume create --size 20 --type type1 examlv1**

**openstack volume show examlv1**

4.通过命令行创建云硬盘，将其设置为只读，查询该云硬盘的详细信息。

**cinder create --display-name "examlv2" --volume-type type1 20**

**cinder readonly-mode-update examlv2 true**

**cinder show examlv2**

5.通过命令行创建云硬盘，查询该云硬盘的详细信息。

**cinder create --display-name "examlv2" --volume-type type1 20**

**cinder show examlv2**

6.使用命令，对/dev/vda 分区，并把这个分区创建成物理卷，然后再把这个物理卷加入到 cinder-volumes 卷组中，查看卷组详情。

**fdisk /dev/vda**

**pvcreate /dev/vda1**

**vgcreate cinder-volume /dev/vda1**

**vgdisplay cinder-volumes**

7.使用命令创建一个云硬盘，然后通过 lvm 相关命令查看该云硬盘的详细信息，最后通过 cinder 命令对这块云硬盘进行扩操作容，并查看详细信息。

**cinder create --display-name "examlv2" --volume-type type1 20**

lvdisplay /dev/centos/cinder--volumes-volume--d85f9089-be7f-4ae0-8b98-6546720ca98a

**cinder extend examlv2 30**

**cinder show examlv2**

8.登录 iaas 云主机，使用命令对硬盘/dev/vda 进行分区，将这个分区创建为物理卷并使用 pvs 查看，然后将这个物理卷添加到 cinder-volumes 卷组中并 使用 vgs 查看。

**parted /dev/sda**

**vgcreate /dev/vda1**

**pvs**

**vgextend  cinder-volumes  /dev/vda1**

**vgs**

9.登录 controller 节点，创建云主机，镜像使用 centos6.5，flavor 使用 m1.medium，配置好网络。然后给云主机 iaas 挂载一个云硬盘，使用这块云硬盘，把云主机 iaas 的根目录扩容，最后在 iaas 云主机上用 df -h 命令查看。

**openstack server create --image centos6.5 --flavor m1.medium --nic net-id=c5221f87-5f27-4fb0-9e31-08269c104676 cindertest**

**openstack server add volume  cindertest  examlv1**

10.登录“iaas-all”云主机，使用命令对磁盘/dev/vda 进行分区，然后使用命令， 创建 raid 磁盘阵列，最后将 md0 格式化为 ext4 格式并查看该磁盘阵列的 UUID。

12.登录“iaas-all”云主机，查看 cinder 后端存储空间大小，将 cinder 存储空间 扩容 10 个 G 大小，最后查看 cinder 后端存储空间大小。

13.修改相应的配置文件，增加 cinder backup 后端备份。

14.配置 NFS 网络存储作为 cinder 的后端存储。

**Swift 运维：**

1.使用命令查看 swift 服务状态，然后创建一个容器，并使用命令查看容器列表。

**openstack-status  | grep swift**

**swift post container1**

**swift list**

2.使用 swift 相关命令，创建一个容器，然后使用命令查看该容器的状态。

**swift post container1**

**swift stat container1**

3.使用 swift 相关命令，查询 swift 对象存储服务可以存储的单个文件大小的最大值。

**swift capabilities**

4.使用 swift 相关命令，创建一个容器，然后往这个容器中上传一个文件（文件可以自行创建），上传完毕后，使用命令查看容器。

**swift post container1**

**swift upload  container1 test.txt**

5.登录 iaas 云主机，使用 openstack 命令，创建一个容器，并查询，上传一个文件（可自行创建）到这个容器中，并查询。

**openstack container create container2**

**openstack container show container2**

**openstack object create container2 test.txt**

**openstack object list container2**

6.登录 IaaS 云主机，创建 swifter 用户，并创建 swift 租户，将 swifter 用户规划到 swift 租户下，赋予 swifter 用户使用 swift 服务的权限，并通过 url 的方式使用该用户在swift中创建容器。

**openstack user create --domain demo --password 000000 swifter**

**openstack project create --domain demo --enable swift**

**openstack role add  --project swift  --user swifter user**

**swift --os-auth-url http://controller:5000/v3 --auth-version 3 --os-project-name swift --os-project-domain-name demo --os-username swifter  --os-user-domain-name demo --os-password 000000  post admin_container3**

7.使用 url 的方式，用 admin 账号在 swift 中创建容器，创建完之后用 url 的 方式查看容器列表。

**openstack role add  --project admin  --user swifter user**

**swift --os-auth-url http://controller:5000/v3 --auth-version 3 --os-project-name swift --os-project-domain-name demo --os-username admin  --os-user-domain-name demo --os-password 000000  post admin_container3**

**swift --os-auth-url http://controller:5000/v3 --auth-version 3 --os-project-name swift --os-project-domain-name demo --os-username swift  --os-user-domain-name demo --os-password 000000  list**

8.配置 swift 对象存储为 glance 的后端存储，并查看。

**KVM 运维：**

1.在物理云平台查询云主机 IaaS 在 KVM 中的真实实例名，在计算节点使用 virsh 命令找到该实例名对应的 domain-id，使用该 domain-id 关闭云主机 IaaS。

**OpenStack server  show  Iaas**

**virsh  dominfo  instance-00000002**

**virsh shutdown d7fe7a00-d0d7-4745-8625-6d88805e4ccf**

2.在物理云平台查询云主机 IaaS 在 KVM 中的真实实例名，在计算节点使用 virsh 命令找到该实例名对应的 domain-id，使用该 domain-id 重启云主机 IaaS。

**OpenStack server  show  Iaas**

**virsh  dominfo  instance-00000002**

**virsh reboot d7fe7a00-d0d7-4745-8625-6d88805e4ccf**

3.此题使用物理 iaas 平台。登录 compute 节点，使用命令将 KVM 进程绑定到特定的 cpu 上。

**ps -e | grep  kvm**

**taskset -cp 0-5 37902**

4.此题使用物理平台。登录 controller 节点，调优 kvm 的 I/O 调度算法，centos7 默认的是 deadline，使用命令将参数改为 noop 并查询。

**echo noop > /sys/block/sda/queue/scheduler**

**cat /sys/block/sda/queue/scheduler**

5.此题使用物理 iaas 平台。登录 controller 节点，使用 cat 命令，只查看当前系统有多少大页，然后设置大页数量并查看，接着使用命令使配置永久生效，最后将大页挂载到/dev/hugepages/上。

**cat /proc/meminfo | grep HugePages**

**echo 2000 > /proc/sys/vm/nr_hugepages**

**sysctl  -w vm.nr_hugepages=2000**

**mount -t hugetlbfs hugetlbfs /dev/hugepages**

6.登录 192.168.100.10/dashboard，创建一个云主机。在云主机所在的物理节点，进入 virsh 交互式界面，调整虚拟机的内存大小，最后使用命令查看该虚拟机的详情。

**virsh**

**virsh #  setmem instance-00000009  3G --config --live**

**virsh # setmem instance-00000009  4G --config --live**

7.KVM 网络优化：让虚拟机访问物理网卡的层数更少，直至对物理网卡的单独占领，和物理机一样的使用物理网卡，达到和物理机一样的网络性能。

**网络运维：**

1.在控制节点安装配置 JDK 环境。安装完成后，查询 JDK 的版本信息。

**mkdir /usr/jdk64**

**tar -zxvf jdk-8u77-linux-x64.tar.gz -C /usr/jdk64**

**vi /etc/profile**

**export JAVA_HOME=/usr/jdk64/jdk1.8.0_77**

**export PATH=$JAVA_HOME/bin:$PATH**

**source /etc/profile**

**java -version**

2.在控制节点安装配置 Maven 环境。安装完成后，查询 Maven 的版本信息。

**mkdir  /data**

**tar -zxvf apache-maven-3.6.2-bin.tar.gz -C /data/**

**vi /etc/profile**

**export MAVEN_HOME=/data/apache-maven-3.6.2**

**export PATH=$MAVEN_HOME/bin:$PATH**

**source /etc/profile**

**mvn -v**

3.继续完成 OpenDaylight 的安装，完成后使用 curl 命令访问网页 http://192.168.100.10:8181/index.html。



4.创建网桥 br-test，把网卡 enp9s0 从原网桥迁移到 br-test，查询 openvswitch 的网桥信息和该网桥的端口信息。

5.创建命名空间 ns。

6.在网桥 br-test 中创建内部通信端口 tap。

7.在命名空间 ns 中配置端口 tap 的地址为 172.16.0.10/24。

8.在命名空间中查询端口 tap 的地址信息。

9.通过 openvswitch 手动运维 openstack 中虚拟主机的通讯信息。

**Ceilometer 运维：**

1.使用 ceilometer 相关命令，查询测量值的列表信息。

**ceilometer meter-list**

2.使用 ceilometer 相关命令，查询给定名称的测量值的样本列表信息。

**ceilometer sample-list -m vcpus**

3.使用 ceilometer 相关命令，查询事件列表信息。

**ceilometer event-list**

4.使用 ceilometer 相关命令，查询资源列表。

**ceilometer resource-list**

5.按以下提供的参数及其顺序，使用 ceilometer 相关命令创建一个新的基于计算统计的告警。以下题目都需在这个基础上完成。

（1）名字为：cpu_hi

（2）测量值的名称为：cpu_util

（3）阈值为：70.0

（4）对比的方式为：大于

（5）统计的方式为：平均值

（6）周期为：600s

（7）统计的次数为：3

（8）转为告警状态时提交的 URL 为：'log://'

（9）关键字：resource_id=INSTANCE_ID

**ceilometer alarm-threshold-create --name high_cpu_util --description 'High CPU Util' --meter-name cpu_util --threshold 70.0  --statistic avg --period 70 --evaluation-periods 2 --alarm-action 'log:// --queryresource id='RESOURCEID''**

6.使用 ceilometer 相关命令，查询用户的告警列表信息。

**ceilometer alarm-list**

7.使用 ceilometer 相关命令，查询给定名称的告警的历史改变信息。

**ceilometer alarm-history  **                          **ceilometer alarm-history a3cccb1d-fea9-4b56-8eab-a743595b3ef2**

8.使用 ceilometer 相关命令，修改给定名称的告警状态为不生效。

**ceilometer alarm-update --enabled False         ceilometer alarm-update --enabled False    a3cccb1d-fea9-4b56-8eab-a743595b3ef2**         

9.使用 ceilometer 相关命令，删除给定名称的告警，并使用命令查看删除结果。

**ceilometer alarm-delete a3cccb1d-fea9-4b56-8eab-a743595b3ef2**

**ceilometer alarm-list**

10.使用Ceilometer相关命令，查看某云主机有哪些样本，然后使用Ceilometer 命令查看云主机的特定样本信息。

**ceilometer sample-list**

**ceilometer sample-show 372645cc-f442-11e9-bb2b-40a8f0282afc**

**Heat 运维：**

1.使用 heat 相关命令，查看 heat 的服务列表信息。

**heat service-list**

2.使用 heat 相关命令，查询给定的详细资源类型信息。

**heat resource-list**

3.使用 heat 相关命令，查询 heat 模板的版本信息。

**heat template-version-list**

4.使用 heat 相关命令，查询 heat 最新版本模板的功能列表。

**heat template-function-list heat_template_version.2016-04-08**

5.使用提供的文件 server.yml 创建名为 heat 的 stack，其中 glance 镜像使用 centos7，网络使用 int-net1。查询 stack 列表信息。

**(heat stack-create --template-file server.yml --parameters "image=centos7;network=int-net2" mystack   此题的标准答案，下面答案是为了与server.yml 相符合所适用的    )**

**heat stack-create -f server.yml -P Image=centos -P Net=int-net2 mystack   //命令不懂看server.yml脚本解释**

**heat stack-list**

6.现有 server.yml 文件，请使用该 yml 文件创建堆栈 mystack，指定使用镜 像 centos6.5，使用网络 int-net1，待创建完成后，查询堆栈 mystack 的状态 为 CREATE_COMPLETE 的事件信息。

**(heat stack-create --template-file server.yml --parameters "image=centos6.5;network=int-net1" mystack   此题的标准答案，下面答案是为了与server.yml 相符合所适用的    )**

**heat stack-create -f server.yml -P Image=examimage -P Net=int-net1 mystack**

**heat event-show mystack server1  8b515891-9a64-441b-abbd-44ec027be47b**

7.对提供的 server.yml 模板进行修改，添加所需参数。通过命令使用 heat 模板创建名为test-heat的stack，其中glance镜像使用centos7，网络使用int-net1。 查询 stack 列表信息。

**数据加密：**

前提：按要求配置静态 fixed_key，使 cinder 和 nova 组件可以使用加密过的 Block Storage 卷服务，配置好之后，创建一个卷类型叫 luks，并把这个类型配置 为加密类型，cipher 使用“aes-xts-plain64”，key_size 使用“512”，control-location 使用“front-end”，Provider 使用“nova.volume.encryptors.luks.LuksEncryptor”。

**openstack-config --set /etc/cinder/cinder.conf keymgr fixed_key HEX_KEY**

**systemctl restart openstack-cinder-volume**

**openstack-config --set /etc/nova/nova.conf keymgr  fixed_key HEX_KEY**

**openstack-service restart nova-compute**

**cinder type-create luks**

**cinder type-create luks**

**cinder encryption-type-create --cipher aes-xts-plain64 --key_size 512 --control_location front-end luks nova.volume.encryptors.luks.LuksEncryptor**

1.使用命令查看卷类型列表和加密卷类型列表。

**cinder type-list**

**cinder encryption-type-list**

2.使用命令创建两个卷，前者不加密，后者使用 luks 卷类型加密。然后查看卷列表。

**cinder create --display-name volume1  --volume-type type1 20**

**cinder create --display-name volume2  --volume-type luks 20**

**openstack volume list**

3.使用命令创建两个卷，前者不加密，后者使用 luks 卷类型加密。使用 nova 命令，创建一个云主机，镜像使用提供的 cirros 镜像，然后使用命令分别将 创建的两块云硬盘 attach 到云主机上，最后使用 cinder list 查看。

**cinder create --display-name volume1  --volume-type type1 20**

**cinder create --display-name volume2  --volume-type luks 20**

**nova boot  --flavor docker --image "examimage"  --nic net-id=c5221f87-5f27-4fb0-9e31-08269c104676 vm-test1**

**nova volume-attach vm-test1 e36e5bd8-167b-466e-829a-6be337761847   （此ID为云硬盘ID）**

**nova volume-attach vm-test2 2ae85634-3e8a-4e95-94dc-a050ffa0cb2c**

**cinder list**

4.使用命令创建两个卷，前者不加密，后者使用 luks 卷类型加密。使用 nova 命令，创建一个云主机，镜像使用提供的 cirros 镜像，然后使用命令分别将创建的两块云硬盘 attach 到云主机上，最后使用 strings 命令验证数据卷的加密功能。

**cinder create --display-name volume1  --volume-type type1 20**

**cinder create --display-name volume2  --volume-type luks 20**

**nova boot  --flavor docker --image "examimage"  --nic net-id=c5221f87-5f27-4fb0-9e31-08269c104676 vm-test1**

**nova volume-attach vm-test1 e36e5bd8-167b-466e-829a-6be337761847   （此ID为云硬盘ID）**

**nova volume-attach vm-test2 2ae85634-3e8a-4e95-94dc-a050ffa0cb2c**

**负载均衡：**

1.安装完 neutron 网络后，使用 neutron 命令查询 lbaas 服务的状态。（物理环境）

**neutron agent-list**

2.使用负载均衡创建 nginx 资源池，使用 http 协议，选择轮循负载均衡方式。 创建完成后添加 vip：nginx-vip，使用 http 协议，端口为 80，HTTP_COOKIE 会话持久化。使用 neutron 命令查询资源池 nginx 详细信息、nginx-vip 详细 信息。

**neutron lb-pool-create --name nginx --lb-method ROUND_ROBIN --subnet-id f9df5c0e-94ba-46f2-89f0-d15ace32aa4b --protocol HTTP**

3.使用负载均衡创建 nginx 资源池，使用 http 协议，选择轮循负载均衡方式。 创建完成后添加 vip：nginx-vip，使用 http 协议，端口为 80，HTTP_COOKIE 会话持久化。使用命令查看所创建资源池的 haproxy 配置文件。（物理环境）

**防火墙：**

1.防火墙规则创建，添加名为 icmp 的规则，拒绝所有源 IP、源端口、目的 IP、目的端口的 ICMP 规则。使用 neutron 命令查询规则列表信息、详细信息。（物理环境）

**neutron firewall-rule-create  --name icmp --protocol icmp --action reject**

**neutron firewall-rule-list**

**neutron firewall-rule-show icmp**

2.防火墙创建，创建名为 nginx 的防火墙，添加防火墙规则 nginx-80，放行 所有源 IP、源端口、目的 IP、目的端口为 80 的规则。创建防火墙策略 nginx-policy，添加 nginx-80 规则。使用 neutron 命令查询防火墙详细信息、策略详细信息、规则详细信息。（物理环境）

**neutron firewall-rule-create --name nginx80 --protocol tcp --destination-port 80 --action allow**

**neutron firewall-policy-create --firewall-rule nginx80  nginx-policy**

**neutron firewall-create nginx-policy --name nginx**

**neutron firewall-policy-show nginx-policy**

**neutron firewall-rule-show nginx80**