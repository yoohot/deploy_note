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
docker version:  
docker info:docker基本信息  
docker --help:帮助信息  

镜像命令：  
docker images [option]：tag 版本标签，imageid 镜像id  
             option：-a 本地所有镜像（含中间映像层）  
                  -q 只象时imageid  
                  --digests：显示摘要信息  
                  --no-trun：不截取，显示完整信息  
docker search [option] image_name：搜索镜像 hub.docker.com 而不是加速器地址   
             option : -s stars数 eg -s 100 超过100stars  
                     --no-trunc:完整信息  
                     --automated：只列出automated build类型镜像即automated列为ok的  
docker pull [option]  image_name[:tag=latest]: 从远程[加速器]仓库下载  
            option: -a 下载所有tag版本  
                    -q 静默下载  
docker rmi [option]  image_name[:tag=latest]:  
         option: -f强制删除   
         删除全部：docker rmi -f $(docker images -qa)  
容器命令  
docker run [option] image [command] [arg...]:创建并运行容器    
           option: -i以交互模式运行容器，通常和-t一起使用    
                   -t为容器重新分配一个伪输入终端，通常和-i一起使用  
                   -d 后台运行，并返回容器id，也即启动守护容器  
                   -P 随机端口映射  
                   -p 指定端口映射 四种格式  
                      ip:hostPort:containerPort  
                      ip::containerPort  
                      hostPort:containerPort  
                      containerPort  
                   --name=XXX 为容器指定名称  
         
docker ps [option]: 查看运行种的容器     
          option:-a 展示全部，包括历史容器  
docker start 容器id：启动容器
docker restart 容器id：重启容器
docker stop 容器id：停止容器
docker kill 容器id：强制停止
docker rm [-f 强制] 容器id:删除已停止的容器 docker rm $(docker ps -qa)  
docker logs [option]容器id：查看容器日志（容器前台进程产生的日志,相当于shell界面日志）  
            option:-f 跟随最新日志   
                   -t 时间   
                   --tail 显示最后    
docker top 容器id：显示容器内部运行进程   
docker inspect 容器id：查看容器内部细节  
进入容器并以命令行交互：  
 docker exec -it 容器id /bin/bash ：exit不会关闭  
 docker attach 容器id：不会启动新的进程,这时exit会导致容器关闭,不推荐该命令  
docker cp 容器id：容器内路径 主机路径：拷贝文件到宿主机  
      
eg: docker run -it --name=myname image 启动交互容器，exit退出后自动关闭     
    docker run -d ... image 以守护进程启动，docker容器后台运行，则容器种必须有一个前    
    台进程，容器种运行的命令若不是一直挂起的命令(如top，tail),就会自动退出所以，最佳的    
    解决方案时，将运行的程序以前台进程的形式运行      


#### 镜像原理  
镜像是以轻量级，可执行的独立软件包，用来打包软件运行环境和基于运行环境开发的软件。它包含  
运行某个软件所需的所有内容，包括代码，运行时，库，环境变量和配置文件。。  
UnionFS：联合文件系统，是一种分层，轻量级且高性能的文件系统，它支持对文件系统的修改作为  
一次提交来一层层的叠加，unionfs是docker镜像基础，镜像可以通过分层来镜像继承，基于基础就   
像（没有父镜像）可以制作各种具体应用镜像；特性，一次加载多个文件系统，但从外面看，只能看  
到一个我呢见系统，联合加载会把哥层文件系统叠加起来，这样最终的文件系统包含所有底层的文件  
和目录，docker镜像实际上由一层一层文件系统组成，这种层级席间系统就是unionfs。

bootfs:(boot file system,主要包含bootloader和kernel,bootloader主要是引导加载kernel   
linux刚启动时会加载bootfs，在docker镜像最底层就是bootfs，这一层与我们经典的linux/unix  
系统一样，包含boot加载器和内核，当boot加载完成之后整个内核就都到内存种了，此时内存的使  
用权由bootfs转交给内核，此时系统也会卸载bootfs   
rootfs:在bootfs之上，包含的就是典型的linux系统种的 /dev /proc /etc等标准目录和文件，
rootfs就是各种不同文件系统的发行版，如centos，ubuntu
分层好处：多个镜像从相同base镜像构建而来，宿主机只需在磁盘保存一分base镜像，内存中也只  
需加载一份base镜像，就可以为多个容器共享  

docker commit [option] 容器id imageName[:标签名]:提交镜像副本，使之成为一个新的镜像
            option:-m='描述信息'
                   -a='auther'


#### 容器数据卷
docker容器产生的数据，如果不用commit生成新的镜像，使得数据成为镜像的一部分  
保存下来，那么当容器被删除时候数据也就丢失。为了能保存数据而使用数据卷。
1. 绕过“拷贝写”系统，以达到本地磁盘IO的性能，（比如运行一个容器，在容器中对
数据卷修改内容，会直接改变宿主机上的数据卷中的内容，所以是本地磁盘IO的性能，
而不是先在容器中写一份，最后还要将容器中的修改的内容拷贝出来进行同步。） 
2. 绕过“拷贝写”系统，有些文件不需要在docker commit打包进镜像文件。 
3. 在多个容器间共享目录 
4. 在宿主和容器间共享目录 
5. 在宿主和容器间共享一个文件。
数据卷添加：
1 命令添加
添加 ：docker run -it --name='xx' ... -v /宿主机绝对路径:/容器内绝对路径 镜像
查看： docker inspect 容器id，其中volumes
数据共享：宿主机或容器中添加
权限:docker run -it --name='xx' .. -v /宿主机绝对路径:/容器内绝对路径:ro 镜像
     #ro read only容器只读，默认rw可读可写
2dockerfile添加
在dockerfile可以使用volume命令来给镜像添加一个或多个数据卷 
出于可移植和分享的考虑，-v 主机目录：容器目录不可在dockerfile中使用，由于宿主机
目录依赖宿主机本身，目录不一定存在
编写Dockerfile
FROM centos
VOLUME ['/containerDir1','containerDir2']
CMD echo 'finished success...'
CMD /bin/bash
构建镜像 docker build -f /Dockerfile -t cgl/centos /dir 这种镜像启动的容器自动带了
数据卷，docker会自动挂载在宿主机的默认目录下/var/lib/docker/volumes/自动生成目录名
docker build [option] targetpath 
             option: -f 指定Dockerfile路径
			         -t  同--tag 镜像tag
					 -v volume
数据卷容器：容器挂载数据卷，其他容器通过挂载这个容器实现数据共享，挂载了
数据卷的容器，我们称之为数据卷容器；上面两种数据卷方式都可以；实际上是数据卷配置的
传递，删除容器不会影响其他容器的数据卷。
 参数 --volumes-from 另一个容器 
eg docker run -it --name dc1 image
   docker run -it --name dc2 volumes-from dc1 image #共享dc1容器的数据卷
   docker run -it --name dc3 volumes-from dc2 image #共享dc2（dc1）数据卷

   
#### Dockerfile解析
dockerfile是用来构建镜像的构建文件，是由一系列命令和参数构成的脚本
编辑dockerfile文件 -> build成镜像 -> run
1每条保留字指令必须大写，且指令后至少需要一个参数
2指令按照从上到下的顺序执行
3#表示注释
4每条指令都会创建一个新的镜像层，并对镜像进行提交
执行流程：
1docker从基础镜像运行一个容器
2执行一条指令并对容器做出修改
3执行类似commit操作提交一个新的镜像层
4docker再基于刚提交的镜像运行一个新容器
5执行dockerfile中的下一个指令循环以上步骤直到所有指令执行完成

dockerfile关键字
FROM:当前镜像基于的基础镜像
MAINTAINER:镜像维护者的名称/邮箱 （格式不固定，就是一个描述）
RUN:用于指定 docker build 过程中要运行的命令,通常用于安装软件包
    eg redis dockerfile中会添加redis用户和用户组
EXPOSE:当前容器对外暴露的端口号
WORKDIR:创建容器后，终端登陆进来默认的工作目录，落脚点，没指定默认 / 
ENV:在构建镜像过程中设置环境变量 key value，这个变量可以在后续其他命令中使用，$key
    eg: ENV dir /usr/tmp 
	    WORKDIR $dir
ADD:将宿主机目录下的文件拷贝进镜像且ADD命令会自动处理URL和加压tar压缩包
COPY:类似于ADD，将文件/目录拷贝到镜像
       构建上下文路径中的文件/目录   新一层镜像的目标路径位置
	   COPY src dest   COPY ["src","dest"]
VOLUME:容器数据卷，用于数据保存和持久化
CMD:指定容器"启动时"要运行的命令，dockerfile可以有多个cmd命令，但是只有最后一个有效，
    cmd会被docker run 命令后面的 command参数替换【如果存在】
ENTRYPOINT:指定容器"启动时"要运行的命令,同cmd一样。与cmd区别不会被忽略，一定会被执行，
           即使运行docker run时指定了其他命令也会被当作参数传递给ENTRYPOINT，之后形成
		   新的命令组合
ONBUILD:当构建一个被继承的dockerfile时运行的命令,父镜像在被子继承后父镜像的onbuild被触发

docker history 镜像： 查看镜像的构建过程
