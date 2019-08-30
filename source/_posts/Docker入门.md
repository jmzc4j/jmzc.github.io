---
title: Docker入门
date: 2019-08-28 17:09:05
tags: docker
categories: 学习笔记
description: docker、docker入门、docker命令、dockfile
---
### what is
- Docker是一个容器管理引擎，是一个轻量级的虚拟化技术；从镜像和容器的角度来定义一个应用；
- 将应用代码和配置等打包成一个可运行的环境（image），从而实现开发和生产环境的完美对接，达到一种一次构建到处运行的目的；
- repository、image、container为其最重要的三个组成部分。
- image是由一层一层的文件系统组成，即UnionFS；其最底层是bootfs；
- image都是只读的，当容器启动时，一个新的可写层被加载到镜像的顶部；这一层通常被称作“容器层”，“容器层”之下的都叫“镜像层”。
- 通过容器数据卷来实现数据持久化和数据共享；

### 安装与卸载
- centos安装步骤

	①卸载旧版本
	``` 
	$sudo yum remove docker \
	docker-client \
	docker-client-latest \
	docker-common \
	docker-latest \
	docker-latest-logrotate \
	docker-logrotate \
	docker-engine
	```
	②安装依赖包
	```
	$sudo yum install -y yum-utils \
	device-mapper-persistent-data \
	lvm2
	```
	③配置仓库地址（这里使用阿里巴巴的国内镜像）
	```
	$sudo yum-config-manager \
    --add-repo \
	https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
	
	#这里可以清理yum缓存文件
	$yum makecache fast
	```
	④安装最新版或者自行选择版本
	```
	#列出docker引擎的可用版本
	$yum list docker-ce --showduplicates | sort -r
	#选择版本进行安装
	#例如查到docker-ce.x86_64  3:18.09.1-3.el7                     docker-ce-stable
	#那么VERSION_STRING就是 docker-ce-18.09.1
	$sudo yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io
	
	#也可以直接安装最新版
	$sudo yum install docker-ce docker-ce-cli containerd.io
	```
	⑤启动
	```
	$sudo systemctl start docker
	```
	⑥测试
	```
	$ sudo docker run hello-world
	
	或者执行docker version
	```
	⑦配置阿里云的镜像加速
	```
	登录阿里云进入控制台，搜索容器镜像服务
	点击镜像加速器，然后找到centos的配置信息，按步骤配置即可
	
	vi /etc/docker/daemon.json
	#这里的镜像ID（u7dhnsts）不同人会不同
	{"registry-mirrors": ["https://u7dhnsts.mirror.aliyuncs.com"]  }
	
	sudo systemctl daemon-reload
	sudo systemctl restart docker
	```
- 卸载
	```
	$ sudo yum remove docker-ce
	$ sudo rm -rf /var/lib/docker
	```
### docker命令
1. `docker --help|docker <command> --help`：查看帮助
1. `docker images` ：查看镜像列表；
1. `docker rmi -f $(docker images -qa)`：删除多个镜像；
1. `docker ps`；查看正在运行的容器；
1. `docker rm -f $(docker ps -qa)`：删除所有的容器；
1. `docker run <containerName|containerID>` ：在镜像上创建一个容器（本地没有则会自动去远程仓库拉去）；    
	```
	OPTIONS说明（常用）：有些是一个减号，有些是两个减号
	--name="容器新名字": 为容器指定一个名称；
	-d: 后台运行容器，并返回容器ID，也即启动守护式容器；
	-i：以交互模式运行容器，通常与 -t 同时使用；
	-t：为容器重新分配一个伪输入终端，通常与 -i 同时使用；
	-P: 随机端口映射；
	-p: 指定端口映射，有以下四种格式
		  ip:hostPort:containerPort
		  ip::containerPort
	 hostPort:containerPort
		  containerPort
	```     
1. `docker search <imageName:tag>`：在仓库上搜索镜像，默认latest；
1. `docker pull <imageName:tag>`：从远程拉取镜像到本地；
1. `docker exec -it /bin/bash`：进入正在运行的容器；
1. `docker commit -m "xxx" -a "xxx" <containerName|containerID> targetImage:tag`：基于容器生成一个新的镜像；
1. `docker inspect <containerName|containerID>`：查看容器的详细信息；
1. `docker info | docker version`：查看docker的简单信息；
1. `docker logs -f <containerName|containerID>`：查看一个容器的历史记录
1. `docker start|stop|restart <containerName|containerID>`：启动、停止、重启容器；
1. `docker kill <containerName|containerID>`：强制停止容器；
1. `docker cp  containerID:path hostPath`：从容器拷贝文件到主机；
1. `docker build -t 新镜像名字:TAG `：从Dockerfile中创建镜像；
1. `docker history 镜像名`：查看镜像的变更历史；

### Dockerfile
- Dockerfile是用来构建Docker镜像的构建文件，是由一系列命令和参数构成的脚本；
- Dockerfile约定
1. 每条保留字指令都必须为大写字母且后面要跟随至少一个参数；
2. 指令按照从上到下，顺序执行；
3. 每条指令都会创建一个新的镜像层，并对镜像进行提交；
- Dockerfile大致流程
1. docker从基础镜像运行一个容器；
2. 执行一条指令并对容器作出修改；
3. 执行类似docker commit的操作提交一个新的镜像层；
4. docker再基于刚提交的镜像运行一个新容器；
5. 执行dockerfile中的下一条指令直到所有指令都执行完成；
- Dockerfile中的关键字
1. `FROM`：基础镜像，当前新镜像是基于哪个镜像的；
1. `MAINTAINER`：镜像维护者的姓名和邮箱地址；
1. `RUN`：容器构建时需要运行的命令；
1. `EXPOSE`：当前容器对外暴露出的端口；
1. `WORKDIR`：指定在创建容器后，终端默认登陆的进来工作目录，一个落脚点；
1. `ENV`：用来在构建镜像过程中设置环境变量；
1. `ADD`：将宿主机目录下的文件拷贝进镜像且ADD命令会自动处理URL和解压tar压缩包；
1. `COPY`：类似ADD，拷贝文件和目录到镜像中；但不进行解压；
1. `VOLUME`：容器数据卷，用于数据保存和持久化工作；
1. `CMD`：指定一个容器启动时要运行的命令；多个CMD会被覆盖；CMD 会被 docker run 之后的参数替换；
1. `ENTRYPOINT`：ENTRYPOINT 的目的和 CMD 一样，都是在指定容器启动程序及参数，以追加的方式执行；
1. `ONBUILD`：当构建一个被继承的Dockerfile时运行命令，父镜像在被子继承后父镜像的onbuild被触发；
- 自定义tomcat9
	```
	FROM         centos
	MAINTAINER    zzyy
	#把宿主机当前上下文的c.txt拷贝到容器/usr/local/路径下
	COPY c.txt /usr/local/cincontainer.txt
	#把java与tomcat添加到容器中
	ADD jdk-8u171-linux-x64.tar.gz /usr/local/
	ADD apache-tomcat-9.0.8.tar.gz /usr/local/
	#安装vim编辑器
	RUN yum -y install vim
	#设置工作访问时候的WORKDIR路径，登录落脚点
	ENV MYPATH /usr/local
	WORKDIR $MYPATH
	#配置java与tomcat环境变量
	ENV JAVA_HOME /usr/local/jdk1.8.0_171
	ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
	ENV CATALINA_HOME /usr/local/apache-tomcat-9.0.8
	ENV CATALINA_BASE /usr/local/apache-tomcat-9.0.8
	ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin
	#容器运行时监听的端口
	EXPOSE  8080
	#启动时运行tomcat
	# ENTRYPOINT ["/usr/local/apache-tomcat-9.0.8/bin/startup.sh" ]
	# CMD ["/usr/local/apache-tomcat-9.0.8/bin/catalina.sh","run"]
	CMD /usr/local/apache-tomcat-9.0.8/bin/startup.sh && tail -F /usr/local/apache-tomcat-9.0.8/bin/logs/catalina.out
	```
### docker安装mysql5.7
```
docker run -d --name mysql-5.7 -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -v /usr/local/mysql/data:/var/lib/mysql -v /usr/local/mysql/my.cnf:/etc/mysql/my.cnf -v /usr/local/mysql/conf.d:/etc/mysql/conf.d -v /usr/local//mysql/mysql.conf.d:/etc/mysql/mysql.conf.d -v /usr/local/mysql/log:/var/log/mysql --privileged=true mysql:5.7

```
