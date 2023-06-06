# Docker入门及安装及基本命令


<!--more-->

# docker简介

Docker 是一个用于开发，交付和运行应用程序的开放平台。Docker 使您能够将应用程序与基础架构分开，从而可以快速交付软件。借助 Docker，您可以与管理应用程序相同的方式来管理基础架构。通过利用 Docker 的方法来快速交付，测试和部署代码，您可以大大减少编写代码和在生产环境中运行代码之间的延迟。

Docker 包括三个基本概念:

 - 镜像（Image）：Docker 镜像（Image），就相当于是一个 root 文件系统。比如官方镜像 ubuntu:16.04 就包含了完整的一套 Ubuntu16.04 最小系统的 root 文件系统。
 - 容器（Container）：镜像（Image）和容器（Container）的关系，就像是面向对象程序设计中的类和实例一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。
 - 仓库（Repository）：仓库可看成一个代码控制中心，用来保存镜像。
Docker 使用客户端-服务器 (C/S) 架构模式，使用远程API来管理和创建Docker容器。

Docker 容器通过 Docker 镜像来创建。

容器与镜像的关系类似于面向对象编程中的对象与类。
![在这里插入图片描述](https://img-blog.csdnimg.cn/33d7590c97d5419690abc80e9bafeced.png)


# 安装docker

Docker 并非是一个通用的容器工具，它依赖于已存在并运行的 Linux 内核环境。

Docker 实质上是在已经运行的 Linux 下制造了一个隔离的文件环境，因此它执行的效率几乎等同于所部署的 Linux 主机。

因此，Docker 必须部署在 Linux 内核的系统上。如果其他系统想部署 Docker 就必须安装一个虚拟 Linux 环境。

如果是Windows或者macOS下安装docker就需要先安装linux虚拟机再安装docker

## centos7安装docker

如果已安装docker，请卸载它们以及相关的依赖项

```
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

### 设置仓库

安装所需的软件包。yum-utils 提供了 yum-config-manager ，并且 device mapper 存储驱动程序需要 device-mapper-persistent-data 和 lvm2。

```
sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```

设置阿里云仓库

```
sudo yum-config-manager \
    --add-repo \
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

### 安装 Docker Engine-Community

```
sudo yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

**要安装特定版本的 Docker Engine-Community，请在存储库中列出可用版本，然后选择并安装：**

1、列出并排序您存储库中可用的版本。此示例按版本号（从高到低）对结果进行排序。

```
yum list docker-ce --showduplicates | sort -r
```

```
输出
docker-ce.x86_64  3:18.09.1-3.el7           docker-ce-stable
docker-ce.x86_64  3:18.09.0-3.el7           docker-ce-stable
docker-ce.x86_64  18.06.1.ce-3.el7           docker-ce-stable
docker-ce.x86_64  18.06.0.ce-3.el7           docker-ce-stable
```

2、通过其完整的软件包名称安装特定版本，该软件包名称是软件包名称（docker-ce）加上版本字符串（第二列），从第一个冒号（:）一直到第一个连字符，并用连字符（-）分隔。例如：docker-ce-18.09.1。

```
$ sudo yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io
```

# 卸载docker

删除安装包：

```
yum remove docker-ce
```

删除镜像、容器、配置文件等内容：

```
rm -rf /var/lib/docker
```

# root下执行命令

docker版本号/信息

```
docker version

docker info
```

启动docker

```
systemctl start docker
```

关闭docker

```
systemctl stop docker
```

重启docker

```
systemctl restart docker
```

docker自启动

```
systemctl enable docker
```

查看docker 运行状态

```
systemctl status docker
```

搜索镜像

```
docker search 镜像名
```

下载镜像（默认下载最新版本）

```
docker pull 镜像名
docker pull 镜像名:tag
```

显示镜像

```
docker images
```

运行镜像

```
docker run 镜像名
docker run 镜像名:Tag
```

启动容器并配置端口映射

- 因为容器是隔离的，默认情况下，我们是无法通过宿主机（安装docker的服务器）端口来直接访问容器的 ,因为docker容器自己开辟空间的端口与宿主机端口没有联系，需要配置端口映射

```
-p 宿主机端口:容器端口
```

```
docker run -d -p 16379:6379 --name redis001 redis
```

删除容器

```
docker rm -f 容器名|容器ID
```

显示运行中的容器

```
docker ps
```

显示所有容器

```
docker ps -a
```

删除没有被容器使用的镜像

```
docker rmi -f 镜像名/镜像ID
```

强制删除镜像

```
docker image rm 镜像名称/镜像ID
```

重命名容器

```
docker rename 容器ID/容器名 新容器名
```

进入容器

```
docker exec -it 容器名/容器ID /bin/bash

#进入 前面的 redis001容器   
docker exec -it redis001 /bin/bash
```

退出容器

```
exit

# 优雅退出 --- 无论是否添加-d 参数 执行此命令容器都不会被关闭
Ctrl + p + q
```

停止容器

```
docker stop 容器ID/容器名
```

重启容器

```
docker restart 容器ID/容器名
```

kill容器

```
docker kill 容器ID/容器名
```

容器日志

```
docker logs -f --tail=要查看末尾多少行 默认all 容器ID
```

数据挂载：将容器内的数据与外部宿主机文件绑定起来，类似一个双持久化，当容器删除时，宿主机文件数据目录仍在，下次启动容器只要将数据目录指向宿主机数据所在位置即可恢复！一个容器可以同时挂载多个文件

```
-v 宿主机文件存储位置:容器内文件位置
```

