# Iaas速搭

![image-20201110112755772](C:\Users\Deus\AppData\Roaming\Typora\typora-user-images\image-20201110112755772.png)



Hostnamectl set-hostname controller

Systemctl stop firewalld

Systemctl disable firewalld

Setenforce 0

Vi /etc/selinux/config

​    Permissive

Mkdir -p /opt/centos /opt/iaas

Mount -o loop centos /opt/centos

Mount -o loop xiandain /opt/iaas

Rm -rf /etc/yum.repos.d/*

Vi /etc/yum.repos.d/local.repo

Scp /etc/yum/repos.d/local.repo 192.168.100.20:/etc/yum/repos.d/

Vi /etc/hosts

Yum install vsftpd -y

Vi /etc/vsftpd/vsftpd.conf

Anon_root/opt

Systemctl restart vsftpd

Systemctl enable vsftpd

 

 

%s/##/@@/g

%s/#//g

%s/@@/##/g

%s/PASS=/&000000 /g

 

Scp /etc/xiandian/openrc.sh compute:/etc/xiandian/openrc.sh





172.16.1.218
000000
lsblk

controller
iaas-pre-host.sh
iaas-install-mysql.sh
iaas-install-keystone.sh
iaas-install-glance.sh
iaas-install-nova-controller.sh
iaas-install-neutron-controller.sh
iaas-install-neutron-controller-gre.sh
iaas-install-dashboard.sh

compute
iaas-pre-host.sh
iaas-install-nova-compute.sh
iaas-install-neutron-compute.sh
iaas-install-neutron-compute-gre.sh




openstack
cd /etc/keystone
# source admin-openrc.sh
# glance image-create --name "CentOS7.0" --disk-format qcow2  --container-format bare --progress <       /opt/images/centos_7-x86_64_xiandian.qcow2    


route
ext-router 外部网络  ext-net  接口  子网int-net:10.0.0.0/24(int-subnet)

network
网络名称
 ext-net   ext-subnet   192.168.200.0/24 网关ip  192.168.200.1

 int-net 1 int-subnet1  10.0.0.0/24 网关ip 10.0.0.1  共享  状态
共享Ture 外部False


云主机 
test
浮动ip配置

访问安全 先删除所以规则   然后    添加规则   
2.2、部署Docker 仓库
2.2.1、上传仓库部署使用的镜像。
# ll
-rw-r--r-- 1 root root   33918976 Oct 17 10:20 registry_latest.tar
# docker load -i registry_latest.tar
# docker images
REPOSITORY           TAG                 IMAGE ID            CREATED             SIZE
docker.io/registry   latest              c9bd19d022f6        15 months ago       33.27 MB
2.2.2、启动仓库容器服务
# docker run -d -p 5000:5000 --restart=always --name registry docker.io/registry:latest
20a07207bf28256d13fbc53cf2a1d978a4827bf8f360b32a8106d996f024c001
# docker ps -a
CONTAINER ID        IMAGE                       COMMAND                  CREATED             STATUS              PORTS                    NAMES
20a07207bf28        docker.io/registry:latest   "/entrypoint.sh /etc/"   8 seconds ago       Up 3 seconds        0.0.0.0:5000->5000/tcp   registry
2.2.3、设置仓库地址
# vi /etc/sysconfig/docker //添加下面两行
ADD_REGISTRY='--add-registry 10.0.3.137:5000'
INSECURE_REGISTRY='--insecure-registry 10.0.3.137:5000'
# systemctl daemon-reload
# systemctl restart docker
# docker info 
注意：两个节点都要添加仓库地址。
# docker images
# docker tag c9bd19d022f6 10.0.3.137:5000/registry:latest
# docker push 10.0.3.137:5000/registry:latest
至此仓库就建立好了，我们需要将所有镜像全部推送到仓库中，提供给其他节点使用。
2.3、部署Rancher-Server服务
2.3.1、上传rancher-server镜像
# ll
-rw-r--r-- 1 root root 1000050176 Jan 29 06:23 rancher_server_v1.6.5.tar
# docker load -i rancher-server_v1.6.5.tar
# docker images
REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
<none>                     <none>              f89070da7581        3 weeks ago         984.9 MB
10.0.3.137:5000/registry   latest              c9bd19d022f6        15 months ago       33.27 MB
docker.io/registry         latest              c9bd19d022f6        15 months ago       33.27 MB
# docker tag f89070da7581 10.0.3.137:5000/rancher/server:v1.6.5
# docker push 10.0.3.137:5000/rancher/server:v1.6.5
2.3.2、启动rancher-server服务
# docker run -d --restart=unless-stopped -p 8080:8080 rancher/server:v1.6.5
2ff52cf39d6f2637ac300e7d430dc828fba99cef4ec118793e91e9d680a16509
# docker ps -a
CONTAINER ID        IMAGE                       COMMAND                  CREATED             STATUS              PORTS                              NAMES
2ff52cf39d6f        rancher/server:v1.6.5       "/usr/bin/entry /usr/"   18 seconds ago      Up 6 seconds        3306/tcp, 0.0.0.0:8080->8080/tcp   modest_turing
20a07207bf28        docker.io/registry:latest   "/entrypoint.sh /etc/"   39 minutes ago      Up 38 minutes       0.0.0.0:5000->5000/tcp             registry

 