## docker
是什么:解决了运行环境和配置问题的容器，方便做持续继承并有助整体发布的容器虚拟化技术。

出现原因:产品从开发到上线，不通的运行环境，再到应用配置，开发和运维需要关心很多，
这对开发和运维来说是一种考验，环境配置如此麻烦，每换一台机器，就要重来一次，费力费时，不
能从根本上解决问题，软件可以带环境安装？docker给了一个标准化的解决方案，也就是说安装的时
候，把原始源接一模一样的复制过来，开发人员利用docker可以消除协作编码时‘我的环境可以正常
工作’的问题。打破代码及应用的观念，从系统环境开始，从底至上打包应用，达到应用程序跨平台
的无缝接轨运作，使用户的app（可以是web应用或数据库应用等等）机器运行环境能够做到一次封
装，到处运行。build,ship and run anyapp,anywhere；将应用运行在docker容器之上，而docker
容器在任何操作系统上都是一致的，这就实现了跨平台，跨服务器，只需要一次配置号运行环境，换
到别的机子上就可以一键部署好，大大的简化了操作。

能干嘛：传统虚拟机缺点略，docker容器不是模拟一个完整的操作系统，而是进行进程隔离，将软件
所需的所有资源打包到一个隔离的容器种，容器与不需要捆绑一整套操作系统，只需要软件工作所需
的资源和设置。一次变得高效轻量并保证部署在任何环境种软件都能始终如一的运行

#### docker基本组成：
 镜像:运行的模板
 容器:镜像启动的实例，一个镜像可以有多个容器，可以把容器看作是一个简易版的linux环境（
      包括root用户，进程空间，用户空间和网络空间等)和运行在其中的应用程序。相互隔离
 仓库:镜像仓库，一个仓库可以多个镜像
 docker client：
 docker dameon：

#### 安装：
centos6.8
1 yum install -y epel-release
2 yum install -y docker-io 安装后配置文件 /etc/sysconfig/docker
3 service docker start 启动docker dameon服务 
4 docker version
centos7
1 https://docs.docker.com/install/linux/docker-ce/centos/
 配置文件/etc/docker/daemon.json（安装后不存在，可手动新建）
2 systemctl start docker启动

#### 镜像加速器
阿里云加速：
  注册一个阿里云账户
  获取加速器地址链接获取地址dev.aliyun.com
  配置docker运行镜像加速器
  systemctl daemon-reload
  重启docker daemon
......

docker run hello-world

#### docker底层原理
docker怎么工作的？
cs架构
为什么docker比vm快？
1有更少的抽象层，不需要实现硬件资源虚拟化，直接使用的物理机资源。因此cpu，
内存等 docker效率明显优势
2docker利用的是宿主机的内核，而不是guest os。

#### docker命令

