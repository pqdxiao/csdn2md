> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/justlpf/article/details/132982953)

> ******Docker****** 是一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的容器中，然后发布到任何流行的 ******Linux****** 机器上，也可以实现虚拟化。容器是完全使用沙箱机制，相互之间不会有任何接口。  
>   
> ****Docker Compose**** 是用于定义和运行多容器 docker 应用程序的工具, compose 通过一个配置文件来管理多个 [docker 容器](https://so.csdn.net/so/search?q=docker%E5%AE%B9%E5%99%A8&spm=1001.2101.3001.7020 "docker容器")。可以使用 docker-compose.yml 脚本来 启动、停止、重启应用，进行 docker 容器的编排和管理。但是 docker compose 并没有实现容器的负载均衡，还需要借助其他工具实现。

下面以 ******CentOS****** 系统为例，介绍如何安装 ******Docker****** 以及 ******Docker Compose******。

******安装 Docker******
---------------------

[最详细的 ubuntu 安装 docker 教程_ubuntu docker_软件测试大空翼的博客 - CSDN 博客](https://blog.csdn.net/Tester_muller/article/details/131440306 "最详细的ubuntu 安装 docker教程_ubuntu docker_软件测试大空翼的博客-CSDN博客")

### ******1. 系统版本******

本文以 Ubuntu20.05 系统为例安装 docker，[Ubuntu 官方下载地址](https://ubuntu.com/download "Ubuntu官方下载地址")。

### ******2. 检查卸载老版本 docker******

ubuntu 下自带了 docker 的库，不需要添加新的源。

但是 ubuntu 自带的 docker 版本太低，需要先卸载旧的再安装新的。

注：docker 的旧版本不一定被称为 docker，docker.io 或 docker-engine 也有可能，所以我们卸载的命令为：

```
$ sudo apt-get remove docker docker-engine docker.io containerd runc

```

如果不能正常卸载，出现如下情况，显示无权限时，需要添加管理员权限才可进行卸载：

![](https://img-blog.csdnimg.cn/img_convert/8a83d02734d1a695ac82c50fd4d0c482.png)

我们就需要使用`sudo apt-get remove docker docker-engine docker.io containerd runc`命令使用 root 权限来进行卸载。

### ******3. 安装步骤******

#### 更新软件包

在终端中执行以下命令来更新 Ubuntu 软件包列表和已安装软件的版本:

```
sudo apt update
sudo apt upgrade
```

#### 安装 docker 依赖

Docker 在 Ubuntu 上依赖一些软件包。执行以下命令来安装这些依赖:

```
sudo apt-get install ca-certificates curl gnupg lsb-release


```

#### 添加 Docker 官方 GPG 密钥

执行以下命令来添加 Docker 官方的 GPG 密钥:

```
curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -


```

******结果如下：******

![](https://img-blog.csdnimg.cn/img_convert/a788990a3e1d55b248ec5e17f1934714.png)

#### 添加 Docker 软件源

执行以下命令来添加 Docker 的软件源:

```
sudo add-apt-repository "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"


```

******注：该命令需要使用 root 权限******

![](https://img-blog.csdnimg.cn/img_convert/15218ffd354f7091f7165e5af46dbf07.png)

#### 安装 docker

执行以下命令来安装 Docker:

```
sudo apt-get install docker-ce docker-ce-cli containerd.io


```

#### 配置用户组（可选）

默认情况下，只有 root 用户和 docker 组的用户才能运行 Docker 命令。我们可以将当前用户添加到 docker 组，以避免每次使用 Docker 时都需要使用 sudo。命令如下：

```
sudo usermod -aG docker $USER


```

******注：重新登录才能使更改生效。******

### ******4. 运行 docker******

#### 验证安装

我们可以通过启动`docker`来验证我们是否成功安装。命令如下：

```
sudo systemctl start docker
sudo systemctl status docker
sudo systemctl enable docker
```

#### ******安装工具******

```
sudo apt-get -y install apt-transport-https ca-certificates curl software-properties-common


```

#### ******重启 docker******

```
sudo systemctl restart docker


```

#### ******验证是否成功******

```
sudo docker run hello-world


```

运行命令后，结果如下：

![](https://img-blog.csdnimg.cn/img_convert/d9ee93915b742d40534f4021c887f887.png)

因为我们之前没有拉取过`hello-world`，所以运行命令后会出现本地没有该镜像，并且会自动拉取的操作。

#### ******查看版本******

我们可以通过下面的命令来查看`docker`的版本

```
sudo docker version


```

结果如下：

![](https://img-blog.csdnimg.cn/img_convert/a2a84a24bb78b4dec58ecb53aa094b02.png)

#### ******查看镜像******

上面我们拉取了 hello-world 的镜像，现在我们可以通过命令来查看镜像，命令如下：

```
sudo docker images


```

结果如下图：

![](https://img-blog.csdnimg.cn/img_convert/9414d39f858b31db5ff06965c2b25b76.png)

出现上述情况，即表示我们成功在 Ubuntu 系统上安装了 docker。

******二、安装 Docker Compose******
-------------------------------

### 安装方式一（use）

docker 官网地址：[Overview of installing Docker Compose | Docker Docs](https://docs.docker.com/compose/install/ "Overview of installing Docker Compose | Docker Docs")

#### 1、检查本地 docker 版本

```
docker version

```

#### 2、docker-compose 版本选择

根据 docker 版本选择对应的 docker-compose 版本。

[docker-compose](https://so.csdn.net/so/search?q=docker-compose&spm=1001.2101.3001.7020 "docker-compose") 官网地址：[Compose file version 3 reference | Docker Docs](https://docs.docker.com/compose/compose-file/compose-file-v3/ "Compose file version 3 reference | Docker Docs")

![](https://img-blog.csdnimg.cn/img_convert/1b7703d6340081d832afbb67dbea70ee.png)

#### 3、安装

官网安装地址：[Install Compose standalone | Docker Docs](https://docs.docker.com/compose/install/other/ "Install Compose standalone | Docker Docs")

```
# github: https://github.com/docker/compose/releases/tag/v2.20.2 
# 国内下载地址：https://gitee.com/smilezgy/compose/releases/tag/v2.20.2
sudo curl -SL \
https://github.com/docker/compose/releases/download/v2.20.2/docker-compose-linux-x86_64 \
-o /usr/local/bin/docker-compose
 
# 或者手动下载, 上传到服务器后执行如下指令(use)
# 在 docker-compose-linux-x86_64 文件同一目录下执行
sudo cp docker-compose-linux-x86_64 /usr/local/bin/docker-compose
```

#### 4、添加可执行权限

```
chmod +x /usr/local/bin/docker-compose

```

#### 5、测试

```
[root@bogon bin]# docker-compose --version
Docker Compose version v2.20.2
```

如果需要删除则执行如下命令

```
rm -rf /usr/local/bin/docker-compose

```

#### 6、Docker Compose 运行项目

要运行 Docker Compose，需要在项目目录中拥有一个`docker-compose.yml`文件。完成以下步骤：

1.  打开终端或命令提示符。
2.  导航到存放`docker-compose.yml`文件的目录。
3.  运行以下命令启动在 compose 文件中定义的容器：

```
docker-compose up

```

> 默认情况下，此命令将启动 compose 文件中指定的所有服务，并在终端中显示它们的日志。  
> 要使用分离模式（在后台）运行容器，可以添加 `-d`标志：

```
# 此命令会启动容器并将控制返回给终端。
docker-compose up -d
```

> 请注意，如果是第一次运行 `docker-compose up`，它将从 Docker Hub 拉取任何必要的 Docker 镜像，然后再启动容器。

### 安装方式二

（1）执行如下命令安装 ******pip3******：

```
yum -y install python3-pip

```

```
pip3 install --upgrade pip -i https://pypi.tuna.tsinghua.edu.cn/simple

```

（2）执行如下命令安装 ******docker-compose******：

```
pip3 install docker-compose -i https://pypi.tuna.tsinghua.edu.cn/simple

```

（3）安装完毕后执行如下命令查看版本：

```
docker-compose version

```

（4）控制台显示如下则表示安装成功：

![](https://img-blog.csdnimg.cn/img_convert/3101aff899e59a99b50154acfab34a7c.png)