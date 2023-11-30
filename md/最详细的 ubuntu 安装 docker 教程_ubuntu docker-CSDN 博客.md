> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/Tester_muller/article/details/131440306)

![](https://img-blog.csdnimg.cn/a0cc1dec07a243808bcd51a54425e89a.png)

Docker 是一种流行的容器化平台，它能够简化应用程序的部署和管理。本文将介绍在 Ubuntu 操作系统上[安装 Docker](https://so.csdn.net/so/search?q=%E5%AE%89%E8%A3%85Docker&spm=1001.2101.3001.7020) 的步骤，以便我们可以开始使用 Docker 来构建和运行容器化应用程序。

[获取更多技术资料，请点击！](https://ykzhl.xet.tech/s/1bmQvf)

#### 系统版本

本文以 Ubuntu20.05 系统为例安装 docker，[Ubuntu 官方下载地址](https://ubuntu.com/download)。

#### 检查卸载老版本 docker

ubuntu 下自带了 docker 的库，不需要添加新的源。  
但是 ubuntu 自带的 docker 版本太低，需要先卸载旧的再安装新的。

注：docker 的旧版本不一定被称为 docker，docker.io 或 docker-engine 也有可能，所以我们卸载的命令为：

```
$ apt-get remove docker docker-engine docker.io containerd runc

```

如果不能正常卸载，出现如下情况，显示无权限时，需要添加管理员权限才可进行卸载：

![](https://img-blog.csdnimg.cn/f0c4d4ec2b57475a820c3031e4645b05.png)  
我们就需要使用`sudo apt-get remove docker docker-engine docker.io containerd runc`命令使用 [root 权限](https://so.csdn.net/so/search?q=root%E6%9D%83%E9%99%90&spm=1001.2101.3001.7020)来进行卸载。

#### 安装步骤

1.  更新软件包

在终端中执行以下命令来更新 Ubuntu 软件包列表和已安装软件的版本:

```
sudo apt update
sudo apt upgrade

```

2.  安装 docker 依赖

Docker 在 Ubuntu 上依赖一些软件包。执行以下命令来安装这些依赖:

```
apt-get install ca-certificates curl gnupg lsb-release

```

3.  添加 Docker 官方 GPG 密钥

执行以下命令来添加 Docker 官方的 GPG 密钥:

```
curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -

```

结果如下：

![](https://img-blog.csdnimg.cn/823b3a9fcf314de380b686071f385f9a.png)

4.  添加 Docker 软件源

执行以下命令来添加 Docker 的软件源:

```
sudo add-apt-repository "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"

```

**注：该命令需要使用 root 权限**

![](https://img-blog.csdnimg.cn/d714625bc7394baf8f6d534ca3a69252.png)

5.  安装 docker

执行以下命令来安装 Docker:

```
apt-get install docker-ce docker-ce-cli containerd.io

```

6.  配置用户组（可选）

默认情况下，只有 root 用户和 docker 组的用户才能运行 Docker 命令。我们可以将当前用户添加到 docker 组，以避免每次使用 Docker 时都需要使用 sudo。命令如下：

```
sudo usermod -aG docker $USER

```

**注：重新登录才能使更改生效。**

#### 运行 docker

我们可以通过启动`docker`来验证我们是否成功安装。命令如下：

```
systemctl start docker

```

**安装工具**

```
apt-get -y install apt-transport-https ca-certificates curl software-properties-common

```

**重启 docker**

```
service docker restart

```

**验证是否成功**

```
sudo docker run hello-world

```

运行命令后，结果如下：

![](https://img-blog.csdnimg.cn/52fc9e6c6cc74cb9a5ff590a8761f6e1.png)

因为我们之前没有拉取过`hello-world`，所以运行命令后会出现本地没有该镜像，并且会自动拉取的操作。

**查看版本**

我们可以通过下面的命令来查看`docker`的版本

```
sudo docker version

```

结果如下：

![](https://img-blog.csdnimg.cn/c4cc99c53bc44b9080e11957c095c613.png)

**查看镜像**

上面我们拉取了 hello-world 的镜像，现在我们可以通过命令来查看镜像，命令如下：

```
sudo docker images

```

结果如下图：

![](https://img-blog.csdnimg.cn/9703513287ab4a3cbf6dfcf0349aaada.png)

出现上述情况，即表示我们成功在 Ubuntu 系统上安装了 docker。

[获取更多技术资料，请点击！](https://ykzhl.xet.tech/s/1bmQvf)