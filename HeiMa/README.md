# Docker
学习目标:
  - 掌握Docker基础知识,理解Docker镜像与容器的概念
  - 完成Docker的安装与启动
  - 掌握Docker镜像与容器相关命令
  - 掌握Tomcat Ngnix等软件的常用应用的安装
  - 掌握docker迁移与备份相关命令
  - 能够运用Dockerfile编写创建容器的脚本
  - 能够搭建与使用docker私有仓库

## 1 Docker简介
# 1.1 什么是虚拟化
  在计算机中,虚拟化(Virtualization)是一种资源管理技术,是将计算机的各种实体资源,如服务器、网络、内存及存储等,予以抽象、转换后呈现出来,打破实体结构间的不可分割的障碍,使用户可以比原本的组态更号的方式来应用这些资源。这些资源的新虚拟部分是不受现有资源的假设方式,地域或物理组态所限制。一般所指的虚拟化资源包括计算能力和资料存储。

  在实际的生产环境中,虚拟化技术主要是用来解决高性能的物理硬件产能过剩和老的旧的硬盘产能过低的重组重用,透明化底层物理硬件,从而最大的利用物理硬件,对资源充分利用

  虚拟化技术种类很多,例如: 软件虚拟化、硬件虚拟化、内存虚拟化、网络虚拟化(vip)、桌面虚拟化、服务虚拟化、虚拟机等等。

# 1.2 什么是Docker
  Docker是一个开源项目,诞生于2013年初,最初是 dotCloud公司内部的一个业余项目。它基于Google公司退出的Go语言实现。项目后来加入了Linux基金会,遵从了Apache2.0 协议,项目代码在Github上进行维护。

  Docker自开源以来收到广泛的关注和讨论,以至于 dotCloud公司后来都改名位  Docker Inc。Redhat已在其RHEL6.5中集中支持Docker; Google也在其PaaS产品中广泛应用。

  Docker项目的目标是实现轻量级的操作系统虚拟化解决方案。 Docker的基础是Linux容器(LXC)等技术。

  在LXC的基础上Docker进行了进一步的封装,让用户不需要去关心容器的管理,使得操作更为简便。用户操作 Docker 的容器就像操作一个快速轻量级的虚拟机一样。

  极大的方便了我们环境的部署.(可以自己从网上下载环境所需的镜像,或者是自己编写镜像发布到网上.保持测试和开发环境的一致性)

  为什么选择Docker?
  (1) 上手快
    用户只需几分钟,就可以把自己的程序"Docker化"。Docker依赖于"写时复制" (copy-on-write)模型,使修改应用程序也非常迅速,可以说达到"随心所致,代码即改"的境界。

    随后,旧可以创建容器来运行应用程序了。大多数Docker容器只需要不到1秒中即可启动。由于去除了管理程序的开销, Docker容器拥有很高的性能, 同时同一台宿主机中也可以运行更多的容器, 使用户尽可能的充分利用系统资源

  (2) 职责的逻辑分类
    使用Docker,开发人员只需要关心容器中运行的应用程序,而运维人员只需要关系如何管理容器。Docker设计的目的就是要加强开发人员写代码的开发环境与应用程序要部署的生产环境一致性。从而降低那种"开发时一切正常,肯定是运维的问题(测试环境都是正常的,上线后出了问题归结为肯定是运维的问题)"

  (3) 快速高效的开发生命周期
    Docker的目标之一就是缩短代码开发、测试到部署、上线运行的周期,让你的应用程序具备可移植性,易于构建,并易于协作。(通俗一点说,Docker就像一个盒子,里面可以装很多物件,如果需要这些物件,可以直接将该大盒子拿走,而不需要从盒子中一件一件的取)

  (4) 鼓励使用面向服务的架构
    Docker还鼓励面向服务的体系结构和微服务架构。Docker推荐单个容器只运行一个应用程序或进程,这样就形成了一个分布式的应用程序模型,在这种模型下,应用程序或者服务都可以表示为一系列内部互联的容器,从而使分布式部署应用程序,扩展或调试应用程序都变得非常简单,同时也提高了程序的内省性。

# 1.3 容器与虚拟机比较
虚拟机: 在一个主机下,它所虚拟出来的操作系统个数是有限的,它将物理资源分给各个操作系统.每个资源是独立的
容器: 可以共享资源,仅在使用的时候才掉用资源。故启动速度更快、占用体级更小

# Docker组件

- 1.4.1 Docker服务器与客户端

Docker是一个客户端-服务器(C/S) 架构程序。 Docker客户端只需要向Docker服务器或者守护进程发出请求,服务器或者守护进程将完成所有工作并返回结果。Docker提供了一个命令行工具Docker以及一整套RESTful API。你可以在同一台宿主主机上运行Docker守护进程和客户端,也可以从本地的docker客户端连接到运行在另一台的远程Docker守护进程。

Docker的守护进程,实际上就是Docker的服务端(主要用来管理Docker的容器,Docker的客户端通过指令来联系Docker的服务端,Docker的服务端操作Docker的容器,从而实现客户端间接的操作容器)

- 1.4.2 Docker的镜像与容器

镜像是构建Docker的基石。用户基于镜像来运行自己的容器。镜像也是Docker生命周期中的"构建部分"。镜像是基于联合文件系统的一种层次结构,由一系列指令一步一步构建出来。例如:
添加一个文件;
执行一个命令;
打开一个窗口。
也可以将镜像当作容器的"源代码"。镜像体级很小,非常"便携",易于分享、存储和更新。

Docker 可以帮助你构建和部署容器,你只需要把自己的应用程序或者服务打包放入容器即可。容器是基于镜像启动起来的, 容器中可以运行一个或多个进程。我们可以认为,镜像是Docker生命周期中的构建或者打包阶段,而容器则是启动或者执行阶段。容器基于镜像启动,一旦容器启动完成后,我们就可以登录到容器中安装自己需要的软件或者服务。

  所以Docker容器就是:
  - 一个镜像格式;
  - 一些列标准操作;
  - 一个执行环境


Docker 借鉴了标准集装箱的概念。标准集装箱将货物运往世界各地,Docker将这个模型运用到自己的设计中, 唯一不同的是: 集装箱运输货物, Docker运输软件。

和集装箱一样, Docker在执行上述操作时, 并不关心容器中到底装了什么, 它不管是web服务器, 还是数据库, 或者是应用程序服务器什么的。 所有的容器都按照相同的方式将内容"装载进去"。


  镜像是用来运行容器的一组文件的集合,实际上是代表容器的模板

# Docker使用国内的源
1. windows下使用 "everything"软件 查找  daemon.json
2. 修改为如下:
````json
{
  "registry_mirrors": ["https://docker.mirrors.ustc.edu.cn"]
}
````


# 查看images(镜像)
````bash
docker images
````
注: 看见的镜像是已经下载好的,因此在没有网络的情况下也可以用

# 搜索镜像
````bash
docker search 镜像名称
````
- 栗子: 搜索centOS
````bash
docker search centos
````

# 拉取镜像
- 作用: 从远程仓库中拉取镜像到本地中
- 语法: docker pull 镜像名称
- 栗子: 拉取 tutum/centos 镜像到本地
````bash
docker pull tutum/centos
````

# 删除镜像
- 按镜像ID删除镜像
````bash
docker rmi 镜像ID
````

- 删除所有镜像
````bash
docker rmi `docker images -q`
````
注: docker images -q 列出了所有


# 查看容器
- 查看正在运行的容器
````bash
docker ps
````
- 查看所有容器
````bash
docker ps -a
````
- 查看最后一次运行的容器
````bash
docker ps -l
````
- 查看停止的容器
````bash
docker ps -f status = exited
````

# 创建与启动容器
- 创建容器:
````bash
docker run
````
- 参数说明:
-i: 表示运行容器
-t: 表示容器启动后会进入其命令行.
-it: 表示容器创建后就能登录进去,即分配了一个伪终端
--name: 为创建的容器命名
-v: 表示目录映射关系,可以使用多个 -v 做多个目录或文件映射
-d: 创建一个守护式容器在后台运行
-p: 表示端口映射,前面是宿主机端口,后面是容器映射的端口.

- 语法:交互式方式创建容器
````bash
docker run -it --name='容器名称' 镜像名称:标签 /bin/bash
````
- 栗子: 创建一个可交互式的centos 7.x 的终端
````bash
docker run -it --name=mycentos centos:7 /bin/bash
````
注:
centos:7在本地images中并不存在,因此会自动从远程仓库中拉取
退回宿主机: exit

- 语法2: 守护式方式创建容器
````bash
docker run -id --name=容器名称 镜像名称:标签
````
- 登录守护式容器方式:
````bash
docker exec -it 容器名称(或者容器ID) /bin/bash
````

# 容器的停止与启动
- 首先查看容器
````bash
docker ps
````
- 语法: 停止容器
````bash
docker stop 容器名称(或者容器ID)
````
栗子: 关闭名称为centos2(假设已开启)的容器
````bash
docker stop centos2
````

- 语法: 启动容器
````bash
docker start 容器名称(或者容器ID)
````
栗子: 打开mycentos(假设存在)容器
````bash
docker start mycentos
````
栗子2: 进入刚才打开的mycentos
````bash
docker exec -it mycentos /bin/bash
````

# 文件拷贝
- 语法: 将文件拷贝到容器内
````bash
docker cp 需要拷贝的文件或目录 容器名称:容器目录
````
- 语法2: 将文件从容器内拷贝出来
````bash
docker cp 容器名称:容器目录 需要拷贝的文件或目录
````

# 目录挂载
在创建容器的时候,将宿主机的目录与容器内的目录进行映射,这样我们就可以通过修改宿主主机某个目录的文件从而取影响容器
- 语法: docker run -id -v 宿主机(docker守护进程所在的机器)目录:容器目录 --name 容器名称 镜像名称:标签
- 栗子: 新建一个守护容器(mycentos3),它使用centos:7的镜像,将本地桌面(C:\Users\Administrator\Desktop\myhtml) 和 容器(/usr/local/myhtml) 之间形成映射
````bash
docker run -it --name=mycentos3 -v C:\Users\Administrator\Desktop\myhtml:/usr/local/myhtml centos:7
````
权限不足,解决方案:
添加如下:
````bash
--privileged=true
````

# 查看容器ip地址
- 语法: docker inspect 容器名称(容器ID)
- 栗子: 查看容器的所有信息
````bash
docker inspect mycentos
````
- 栗子2: 查看容器(mycentos)的ip地址
````bash
docker inspect --format='{{.NetworkSettings.IPAddress}}' mycentos
````

# 容器的删除
- 语法: docker rm 容器名称(容器ID)
- 栗子: 删除mycentos
````bash
docker rm mycentos
````
注: 容器必须处于暂停状态才能删除



## 应用部署
# MySQL部署
- (1) 拉取镜像
````bash
docker pull centos/mysql-57-centos7
````

- (2) 创建容器
````bash
docker run -id --name=tensquare_mysql -p 33306:3306 -e MYSQL_ROOT_PASSWORD=123456  centos/mysql-57-centos7
````
参数说明:
-p: 端口映射, 宿主机端口:容器运行的端口
-e: 添加环境变量, "MYSQL_ROOT_PASSWORD" 是root 用户的登录密码

- (3) 远程登录mysql
使用SQLyog连接

# tomcat部署
(1) 拉取镜像
````bash
docker pull tomcat
````
(2) 创建容器
````bash
docker run -id --name=mytomcat -p 9000:8080 -v C:\Users\Administrator\Desktop\webapps:/usr/local/tomcat/webapps tomcat
````
注:
"-p": 端口映射
"-v": 文件映射

# Nginx部署
(1) 拉取镜像
````bash
docker pull ngix
````
(2) 创建Nginx容器
````bash
docker run -id --name=mynginx -p 80:80 nginx
````

## 迁移与备份
# 容器保存为镜像
- 语法: docker commit 容器名称 镜像名称
- 栗子: 将容器 mynginx 保存为镜像 mynginx_i
````bash
docker commit mynginx mynginx_i
````

# 镜像备份
- 语法: docker save -o 备份名称 镜像名称
- 栗子: 将镜像 mynginx_i 备份为 mynginx.tar (tar类型文件)
````bash
docker save -o mynginx.tar mynginx_i
````

# 镜像回复与迁移
- 语法: docker load -i 镜像(tar文件)
- 栗子: 将 mynginx.tar 文件恢复为镜像
````bash
docker load -i C:\Users\Administrator\mynginx.tar
````

# Dockerfile
# 什么是Dockerfile
- Dockerfile是由一系列命令和参数构成的脚本,这些命令应用于基础镜像并创建一个新的镜像。

1. 对于开发人: 可以为开发团队提供一个完全一致的开发环境;
2. 对于测试人员: 可以直接拿开发时所构建的镜像或者通过Dockerfile文件构建一个新的镜像开始工作了
3. 对于运维人员: 在部署时, 可以实现应用的无缝移植

# 常用命令:

- "FROM image_name:tag": 定义了使用哪个基础镜像启动构建流程

- "MAINTAINER user_name": 声明镜像的创建者

- "ENV key value": 设置环境变量(可以写多条)

- "RUN command": 是Dockerfile的核心部分(可以写多条)

- "ADD source_dir/file dest_dir/file": 将宿主机的文件复制到容器内,如果是一个压缩文件,将会在复制后自动解压

- "COPY source_dir/file dest_dir/file": 和ADD相似, 但是如果有压缩文件并不能解压

- "WORKDIR path_dir": 设置工作目录

# Dockerfile构建jdk1.8镜像
- [1] 创建工作目录(windows下路径,C:\Users\Administrator\Desktop\dockerjdk8)
- [2] 准备Dockerfile文件
- [3] 构建
语法: docker build -t="镜像名称" 文件目录
栗子:
````bash
docker build -t="jdk1.8" C:\Users\Administrator\Desktop\dockerjdk8
````
- [4] 查看
````bash
docker images
````
会发现多了一个jdk1.8

## Docker私有仓库

# 私有仓库搭建与配置
- [1] 拉取私有仓库镜像
````bash
docker pull registry
````

- [2] 根据仓库镜像创建自己的容器
1. 栗子: 创建一个自己的仓库容器
````bash
docker run -id --name=registry -p 5000:5000 registry
````
2. 创建完毕后,使用以下命令检查:
````bash
docker ps
````

3. 打开浏览器,输入 localhost:5000/v2/_catalog,若出现 {"repositories":[]}, 代表仓库容器运行正常,此时的私有仓库为空

- [3] 修改配置文件,让docker信任本地的私有仓库
1. 打开daemon.json (C:\Users\Administrator\.docker\daemon.json)
2. 添加如下内容
````json
"insecure-registries": ["localhost:5000"],
````
3. 修改daemon.json后重启docker服务

# 将本地镜像上传到私有仓库
- [1] 给本地镜像打标签
````bash
docker tag 镜像名称 指定地址/镜像名称
````
栗子:
````bash
docker tag jdk1.8 localhost:5000/jdk1.8
````

- [2] 将标签后的镜像推动私有仓库
````bash
docker push 192.168.1.16:5000/jdk1.8
````





