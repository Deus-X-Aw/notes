## Paas

### 1.环境配置

1.yum install -y policycoreutils-python

2.vi /etc/selinux/config

​	SELINUX=disabled

​	getenforce 

​	disabled

3.systemctl stop firewalld

​	systemctl disable firewalld

4.iptables -F

​	iptables -X

​	iptables -Z

5./usr/sbin/iptables-save

6.vi /etc/sysctl.conf

​	net.ipv4.ip_forward=1

​	net.ipv4.conf.default.rp_filter=0

​	net.ipv4.conf.all.rp_filter=0

7.sysctl -p

8.server节点 	vi /etc/hosts  

​	两个ip

9.server节点

​	yum install -y vsftpd

​	vi /etc/vsftpd/vsftpd.conf

​	anon_root=/opt



​	systemctl restart vsftpd

​	systemctl  enable vsftpd



​	mkdir -p /opt/paas

​	rm -rf /etc/yum.repo/*

​	mount -o loop XianDian-Pass-v2.2.iso /opt/paas



​	vi  /etc/yum.repos.d/docker.repo

​	[docker]

​	name=docker

​	baseurl=file:///opt/paas

​	gpgcheck=0

​	enabled=1

​	

​	yum clean all

​	yum list 

### 2.Docker服务

1.yum install -y docker

​	systemctl restart docker

​	systemctl enable docker

2.部署Docker仓库-

​	cd /opt/paas/

​	find registry_latest.tar

​	docker images

​	docker load -i regaistry_latest.tar

3.启动仓库容器服务

​	docker run -d -p 5000:5000 --restart=always --name registry   docker.io/registry:latest

​	docker ps -a

4.设置仓库地址

##两个仓库都要设置地址

​	vi /etc/sysconfig/docker

​	ADD_REGISTY='--add-registry  (server_ip)x.x.x.x:5000'

​	INSECURE_REGISTRY='--insecure-registry (server_ip)x.x.x.x:5000'

​	systemctl daemon-reload

​	systemctl restart docker



​	docker info



​	

​	docker images

​	docker tag <none>  x.x.x.x:5000/registry:latest

​	docker push x.x.x.x:5000/registry:latest

1、获取仓库类的镜像：

[root@shanghai docker]# curl -XGEThttp://192.168.1.8:5000/v2/_catalog

{"repositories":["nginx"]}

### 3.上传Rncher-Server

1.cd /opt/paas/docker/rancher1.6.5

​	ls

2.docker images

​	docker tag repository x.x.x.x:5000/rancher/server:v1.6.5

​	docker push x.x.x.x:5000/rancher/server:v1.6.5

3.docker run -d --restart=unless-stopped -p 8080:8080 rancher/server:v1.6.5

​	docker ps -a

systemctl stop firewalld.serice



4.进入网页

​	serverIP:8080

