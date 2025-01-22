# 第一部分 宏观的东西

## 第一章 30,000 英尺的容器

> 这一章节中，我们可以学到：
>
> 1. 为什么我们有容器
> 2. 容器可以为我们做什么
> 3. 我们可以在哪里使用他们



### 容器

容器模型的一个特性是，每个容器都共享它所运行的主机的操作系统。这意味着单个主机可以运行比虚拟机更多的容器。

容器也比虚拟机更快、更便携。



### Linux 容器

现代容器背后的一些主要技术包括；内核名称空间、控制组（cgroups）、功能等等。

我知道许多类似容器的技术早于Docker和现代容器。然而，它们都没有像Docker那样改变世界。

几乎所有的容器都是Linux容器。这是因为Linux容器更小、更快，而且Linux有更多的工具。

本书的这个版本中的所有示例都是Linux容器。

*WSL 2 (Windows Subsystem for Linux)* 



### 关于 Mac 容器

没有Mac容器这种东西。

它通过在Mac上的轻量级Linux虚拟机中运行Docker来工作。其他工具，如 Podman 和 Rancher Desktop，也非常适合在 Mac 上使用容器。

Docker 和容器生态系统正在适应Wasm应用程序，你应该期待未来vm、容器和Wasm应用程序在大多数云和应用程序中并行运行。



### WebAssembly 是什么

WebAssembly （Wasm）是一种现代的二进制指令集，它构建的应用程序比容器更小、更快、更安全、更可移植。你用你最喜欢的语言编写应用程序，并将其编译为Wasm二进制文件，它将在任何有Wasm运行时的地方运行。



### Kubernetes 是什么

术语：容器化应用程序是作为容器运行的应用程序。稍后我们将详细介绍这一点。

重要的是要知道所有 Docker 容器都在 Kubernetes 上工作。

如果你需要学习 Kubernetes，请查看以下资源：

- **Quick Start Kubernetes:** 大约100页，将使您在一天内快速了解Kubernetes ！
- **The Kubernetes Book**：这是掌握 Kubernetes 的终极书籍。



### 总结

然而，随着VMware和虚拟机管理程序的成功，出现了一种更新、更高效、可移植的虚拟化技术，称为容器。

WebAssembly 正在推动云计算的第三波浪潮，但 Docker 和容器生态系统正在演变，以与WebAssembly一起工作，本书有一整章专门讨论Docker和WebAssembly。



## 第二章 Docker和容器相关的标准和项目

Docker平台旨在**使构建、运输和运行容器尽可能简单。**

Docker 平台主要有两个部分：

- The CLI (client)
- The engine (server)

CLI 是我们熟悉的 docker 命令行工具，用于部署和管理 container。它将简单的命令转换为 API 请求，并将其发送给引擎。

引擎包含运行和管理容器的所有服务器端组件。

下图展示高级架构，client 和 引擎 可以是在同一个主机或通过网络连接：

![](img/1733698130733.jpg)

图2.2 向您展示了引擎背后的一些复杂性

<img src="img/1733793936026.jpg" style="zoom:47%;" />



### 容器相关标准与项目

- The OCI（**The Open Container Initiative (OCI)**）
- The CNCF（**The CloudNative Computing Foundation (CNCF)**）
- The Moby Project





## 第三章 Getting Docker

三种安装方式：

- Docker Desktop 推荐使用
- Multipass
- Server installs on Linux



### Docker Desktop

与容器一起工作的最好的方式。

可以得到：引擎，漂亮的 UI，所有最新的插件和特征，以及带有市场的扩展系统。如果你想学习Kubernetes，你甚至可以得到Docker Compose和单节点Kubernetes集群。

如果你的公司有超过250名员工或者年收入超过1000万美元，那么你需要支付许可费。



## 第四章 全局观

给到一些亲身经历经验以及 images 与 containers 高层次的视角。

分为 Ops 视角与 Dev 视角

Ops 关注视角：启动、停止、删除、在其中执行命令。

Dev 视角：更多地关注应用程序方面，并通过获取应用程序源代码，将其构建到容器镜像中，并将其作为容器运行。

### Ops 视角

- 检查 Docker 是否正常工作
- 下载一个镜像
- 从镜像启动容器
- 在容器中执行一个命令
- 删除容器

运行 `docker version` 确保 client and engine 都安装并运行。

~~~shell
h.a.hu@AMAR64XLRH7PP K8S-Oceam % docker version
Client:																							<<---- Start of client response
 Version:           27.3.1													-----┐
 API version:       1.47																 |
 Go version:        go1.22.7													   | Client info block
 Git commit:        ce12230													  	 |
 Built:             Fri Sep 20 11:38:18 2024				  	 |
 OS/Arch:           darwin/arm64										  	 |
 Context:           desktop-linux	  							 	-----┘

Server: Docker Desktop 4.36.0 (175267)							<<---- Start of server response
 Engine:
  Version:          27.3.1
  API version:      1.47 (minimum version 1.24)
  Go version:       go1.22.7
  Git commit:       41ca978
  Built:            Fri Sep 20 11:41:19 2024
  OS/Arch:          linux/arm64
  Experimental:     false
 containerd:
  Version:          1.7.21
  GitCommit:        472731909fa34bd7bc9c087e4c27943f9835f111
 runc:
  Version:          1.1.13
  GitCommit:        v1.1.13-0-g58aa920
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
~~~



#### 下载一个镜像

镜像是一个包含 app 需要运行的所有实行的对象。包含：OS 文件系统、应用和所有依赖。

如果你在运营部工作，他们很像一个 VM 模版。如果你是开发者，他们类似于 classes。

~~~shell
h.a.hu@AMAR64XLRH7PP K8S-Oceam % docker images
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
~~~

将新映像复制到 Docker 主机上称为拉取。

> 如果不修改镜像源，那么需要搭梯子下载。

~~~shell
h.a.hu@AMAR64XLRH7PP K8S-Oceam % docker pull ubuntu:latest
latest: Pulling from library/ubuntu
8bb55f067777: Pull complete 
Digest: sha256:80dd3c3b9c6cecb9f1667e9290b3bc61b78c2678c02cbdae5f0fea92cc6734ab
Status: Downloaded newer image for ubuntu:latest
docker.io/library/ubuntu:latest
~~~

运行 docker images 确认拉取命令是否工作：

~~~shell
h.a.hu@AMAR64XLRH7PP K8S-Oceam % docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
ubuntu       latest    20377134ad88   4 weeks ago   101MB
~~~

如果你拉取一个应用程序容器，比如 nginx:latest，你会得到一个带有最小操作系统和运行 nginx 应用程序的代码的映像。



#### 从镜像启动一个容器

~~~shell
h.a.hu@AMAR64XLRH7PP K8S-Oceam % docker run --name test -it ubuntu:latest bash
root@eaf9fa4f38a1:/# 
~~~

**docker run** 告诉 Docker 开启一个容器。

**--name**：标志 Docker 调用这个容器叫 test。

**-it**：标志告诉 Docker 使容器具有交互性，并将您的 shell 附加到容器的终端。

**ubuntu:latest**：基于这个镜像。

**bash**L：告诉 Docker 启动一个Bash shell作为容器的主应用。



从容器内部运行ps命令，列出所有正在运行的进程：

~~~shell
root@eaf9fa4f38a1:/# ps -elf
F S UID        PID  PPID  C PRI  NI ADDR SZ WCHAN  STIME TTY          TIME CMD
4 S root         1     0  0  80   0 -  1074 do_wai 01:10 pts/0    00:00:00 bash
4 R root         9     1  0  80   0 -  1919 -      01:15 pts/0    00:00:00 ps -elf
~~~

- PID 1 是我们告诉容器运行的 Bash 进程
- PID 9 是我们用来列出正在运行的进程的 `ps -elf` 命令

按 `Ctrl-PQ` 退出容器而不终止容器。



~~~shell
h.a.hu@AMAR64XLRH7PP ~ % docker ps
CONTAINER ID   IMAGE           COMMAND   CREATED         STATUS         PORTS     NAMES
eaf9fa4f38a1   ubuntu:latest   "bash"    8 minutes ago   Up 8 minutes             test
~~~



#### 在容器内执行命令

~~~shell
h.a.hu@AMAR64XLRH7PP K8S-Oceam % docker attach test
root@eaf9fa4f38a1:/# 
~~~



#### 删除容器

**docker ps**：验证容器是否在运行

~~~shell
h.a.hu@AMAR64XLRH7PP K8S-Oceam % docker ps 
CONTAINER ID   IMAGE           COMMAND   CREATED      STATUS        PORTS     NAMES
eaf9fa4f38a1   ubuntu:latest   "bash"    3 days ago   Up 1 second             test
~~~

**docker stop**：停止容器

~~~shell
h.a.hu@AMAR64XLRH7PP K8S-Oceam % docker stop test
test
~~~

**docker rm**：删除容器

~~~shell
h.a.hu@AMAR64XLRH7PP K8S-Oceam % docker rm test
test
~~~

**docker ps -a**：验证容器是否成功删除

~~~shell
h.a.hu@AMAR64XLRH7PP K8S-Oceam % docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
~~~



### Dev 视角

- 从 GitHub repo 克隆一个 app
- 检查应用 Dockerfile
- 使 app 容器化
- 将 app 作为容器运行

克隆 psweb APP

~~~shell
h.a.hu@AMAR64XLRH7PP Docker % git clone https://github.com/nigelpoulton/psweb.git
Cloning into 'psweb'...
remote: Enumerating objects: 85, done.
remote: Counting objects: 100% (39/39), done.
remote: Compressing objects: 100% (19/19), done.
remote: Total 85 (delta 28), reused 21 (delta 19), pack-reused 46 (from 2)
Receiving objects: 100% (85/85), 17.14 KiB | 1.56 MiB/s, done.
Resolving deltas: 100% (33/33), done.
~~~



#### 检查应用 Dockerfile

Dockerfile 是一个纯文本文档，它告诉 Docker 如何将应用程序和依赖项构建成一个图像。

**cat Dockerfile**：列出 Docker 内容

~~~shell
h.a.hu@AMAR64XLRH7PP psweb % cat Dockerfile 
# Test web-app to use with Pluralsight courses and Docker Deep Dive book
FROM alpine
LABEL maintainer="nigelpoulton@hotmail.com"
# Install Node and NPM
RUN apk add --update nodejs npm curl
# Copy app to /src
COPY . /src
WORKDIR /src
# Install dependencies
RUN  npm install
EXPOSE 8080
ENTRYPOINT ["node", "./app.js"]
~~~



#### 使 app 容器化

通过使用 `docker build` 创建一个 docker image 叫做  `test:lates`。

> 需要科学上网

~~~shell
h.a.hu@AMAR64XLRH7PP psweb % docker build -t test:latest .
[+] Building 24.7s (10/10) FINISHED                                                                                                               docker:desktop-linux
 => [internal] load build definition from Dockerfile                                                                                                              0.0s
 => => transferring dockerfile: 363B                                                                                                                              0.0s
 => [internal] load metadata for docker.io/library/alpine:latest                                                                                                  7.4s
 => [internal] load .dockerignore                                                                                                                                 0.0s
 => => transferring context: 2B                                                                                                                                   0.0s
 => [1/5] FROM docker.io/library/alpine:latest@sha256:21dc6063fd678b478f57c0e13f47560d0ea4eeba26dfc947b2a4f81f686b9f45                                            1.7s
 => => resolve docker.io/library/alpine:latest@sha256:21dc6063fd678b478f57c0e13f47560d0ea4eeba26dfc947b2a4f81f686b9f45                                            0.0s
 => => sha256:21dc6063fd678b478f57c0e13f47560d0ea4eeba26dfc947b2a4f81f686b9f45 9.22kB / 9.22kB                                                                    0.0s
 => => sha256:cf7e6d447a6bdf4d1ab120c418c7fd9bdbb9c4e838554fda3ed988592ba02936 1.02kB / 1.02kB                                                                    0.0s
 => => sha256:44a37b14f342fc55eba59c5d770a49fda0267c1cdd8a45d3ceff8ce088a0be6a 597B / 597B                                                                        0.0s
 => => sha256:cb8611c9fe5154550f45e284cf977cda4e2b2fee3478552eee31d84be3c95003 3.99MB / 3.99MB                                                                    1.5s
 => => extracting sha256:cb8611c9fe5154550f45e284cf977cda4e2b2fee3478552eee31d84be3c95003                                                                         0.1s
 => [internal] load build context                                                                                                                                 0.1s
 => => transferring context: 53.76kB                                                                                                                              0.1s
 => [2/5] RUN apk add --update nodejs npm curl                                                                                                                    8.0s
 => [3/5] COPY . /src                                                                                                                                             0.0s 
 => [4/5] WORKDIR /src                                                                                                                                            0.0s 
 => [5/5] RUN  npm install                                                                                                                                        7.3s 
 => exporting to image                                                                                                                                            0.2s 
 => => exporting layers                                                                                                                                           0.2s 
 => => writing image sha256:fa669780f4b4146c1b5c79b2e782dfd23809c41059695ef341a389795d972b76                                                                      0.0s 
 => => naming to docker.io/library/test:latest                
~~~

使用 docker images 校验：

~~~shell
h.a.hu@AMAR64XLRH7PP psweb % docker images
REPOSITORY   TAG       IMAGE ID       CREATED              SIZE
test         latest    fa669780f4b4   About a minute ago   105MB
ubuntu       latest    20377134ad88   4 weeks ago          101MB
~~~



#### 在容器中执行一个命令

~~~shell
h.a.hu@AMAR64XLRH7PP psweb % docker run -d \
  --name web1 \
  --publish 8080:8080 \
  test:latest
414a2edad0d9a28e0f1454a304c0a2a7a952ea1c6ed17d3f6283fad5412c5fc5
~~~

打开浏览器，输入 localhost:8080，网页中返回：

~~~html
Hello Docker learners!!!
Check out my other books
Quick Start KubernetesThe Kubernetes BookAI Explained

Be careful. The last time I updated the packages in this app was Dec 2024.
~~~



#### 清除

终止 app，直接删除这个 container

~~~shell
h.a.hu@AMAR64XLRH7PP psweb % docker rm web1 -f
web1
~~~

删除镜像

~~~shell
h.a.hu@AMAR64XLRH7PP psweb % docker rmi test:latest
Untagged: test:latest
Deleted: sha256:fa669780f4b4146c1b5c79b2e782dfd23809c41059695ef341a389795d972b76
~~~





## 第二部分 技术的东西

## 第五章 Docker 引擎

本章内容：

- Docker Engine – The TLDR
- The Docker Engine
- The influence of the Open Container Initiative (OCI) • runc
- containerd
- Starting a new container (example)
- What’s the shim all about
- How it’s implemented on Linux



### **Docker Engine – The TLDR**

Docker Engine 是 Docker 服务端组建的术语，运行和管理容器。

在许多方面，Docker Engine 很像 小汽车引擎：

- 汽车发动机是由许多专门的部件组成的，这些部件共同组成了汽车驱动装置——进气歧管、节气门、汽缸、活塞、火花塞、排气歧管等等。
- Docker引擎是由许多专门的工具组成的，它们一起工作来创建和运行容器——API、镜像构建器、高级运行时、低级运行时、shims等。

下图展示了 Docker 引擎 创建和运行容器。但是是一个简单的关注在开始和运行容器的组建图。

![](img/1735511582013.jpg)

`r` 来指代 `runc`, `c` 来指代 `containerd`



### The Docker Engine

Docker 有两个主要的组件：

- The Docker daemon (sometimes referred to as just “the daemon”)
- LXC

守护进程是一个单片二进制文件，包含API、映像构建器、容器执行、卷、网络等的所有代码。

LXC 完成了与 Linux 内核接口的艰苦工作，并构建了构建和启动容器所需的名称空间和cgroup。



### 替换 LXC

依赖 LXC 给 Docker 项目带来了几个问题。

- 首先，LXC是特定于 linux 的，而Docker希望成为多平台的。
- 其次，Docker 发展很快，没有办法确保 LXC 按照Docker需要的方式发展。
- 为了改善体验并帮助项目更快地发展，Docker用自己的工具 **libcontainer** 取代了LXC。libcontainer 的目标是成为一个平台无关的工具，使Docker能够访问主机内核中的基本容器构建块。

Libcontainer 很早以前就取代了 Docker 中的 LXC。



### 分解单片Docker守护进程

Docker 引擎最初是一个庞然大物，几乎所有的功能都被编码到守护进程中。会有一些问题：

- 他变慢了
- 这不是生态系统想要的
- 很难在单一软件上进行创新

长期运行的项目来分解和重构引擎：每个功能都成为自己的小型专用工具。平台构建者可以重用这些工具来构建其他平台。

下图显示了 Docker 引擎中用于运行容器的主要组件的另一个视图，并列出了每个组件的主要职责。

![](img/1735512830450.jpg)



### 开放容器倡议（OCI - Open Container Initiative）的影响

大约在Docker公司重构引擎的同时，[OCI8](https://opencontainers.org/) 也在定义两个与容器相关的标准：

- Image Specification (image-spec)[https://github.com/opencontainers/image-spec]
- Runtime Specification (runtime-spec)[https://github.com/opencontainers/runtime-spec]

稳定性是低级 OCI 规范的核心。

自2016年以来，Docker的所有版本都实现了OCI规范。例如，Docker 使用 runc （OCI运行时规范的参考实现）来创建OCI兼容的容器（运行时规范）。它还使用 BuildKit 来构建OCI兼容的映像（image-spec）， Docker Hub是一个OCI兼容的注册表（registry- spec）。



### runc

runc 是 libcontainer 的轻量级 CLI 包装器，您可以下载并使用它来管理兼容oci的容器。

Docker 和 Kubernetes 都使用 runc 作为默认的低级运行时，并将其与容器高级运行时配对：

- containerd 作为管理生命周期事件的高级运行时运行
- runc 作为低级运行时运行，通过接口执行生命周期事件使用内核来完成实际构建容器和删除它们的工作

你可以看到最新的版本：https://github.com/opencontainers/runc/releases/tag/v1.2.3



### containerd

containerd（发音为“container dee”，总是用小写的“c”写）是Docker在剥离守护进程功能时创建的另一个工具。

它使用的 shims 使得用其他低级运行时替换 runc 成为可能。

你可以看到最新的版本：https://github.com/containerd/containerd/releases



### Starting a new container - 例子

运行以下命令，根据 nginx 映像启动一个名为 ctr1 的新容器。

~~~shell
$ docker run -d --name ctr1 nginx
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
f5c6876bb3d7: Pull complete 
bdb964b66a74: Pull complete 
b5368be906ec: Pull complete 
755bf136756e: Pull complete 
aafc2e219c20: Pull complete 
261c6a94b398: Pull complete 
a8066ea4829b: Pull complete 
Digest: sha256:42e917aaa1b5bb40dd0f6f7f4f857490ac7747d7ef73b391c774a41a8b994f15
Status: Downloaded newer image for nginx:latest
35e13fb8507f04cdd1e0013121ffc0a8652e8eb8b2f63b7d93861b733d74f392

$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS     NAMES
35e13fb8507f   nginx     "/docker-entrypoint.…"   25 seconds ago   Up 25 seconds   80/tcp    ctr1
~~~

当您运行这样的命令时，Docker 客户端将它们转换为 API请求，并将它们发送到守护进程公开的 API。

守护进程可以在本地套接字上或通过网络公开API。Linux操作系统下，本地套接字为 `/var/run/docker`。在Windows上，它是`\pipe\docker_engine`。

![](img/1735519864881.jpg)

我们有时称其为**无守护进程容器(*daemonless containers*)**。

如果您之前启动了NGINX容器，您应该使用以下命令删除它。

~~~shell
 $ docker rm ctr1 -f
~~~



### What’s the shim all about?

Shims是一种流行的软件工程模式，Docker引擎使用他们在容器层和OCI层之间，带来以下好处：

- Daemonless containers
- Improves efficiency
- Makes the OCI layer pluggable

在效率方面，containd为每个新容器划分了一个流程和一个运行流程。但是，一旦容器开始运行，每个runc进程就会退出，使shim进程成为容器的父进程。

shim 是轻量级的，位于 containerd 和 container 之间。它报告容器的状态并执行低级任务，例如保持容器的 STDIN 和 STDOUT 流打开。

shim 还使得用其他低级运行时替换 runc 成为可能。



### How it’s implemented on Linux

- /usr/bin/dockerd (the Docker daemon)
- /usr/bin/containerd
- /usr/bin/containerd-shim-runc-v2
- /usr/bin/runc



### Do we still need the daemon

在撰写本文时，Docker已经剥离了守护进程的大部分功能，使守护进程专注于服务API。



## 第六章 Working with Images

这一章节会深入了解 Docker Images。

### Docker images – The TLDR

下面所有的术语都是同一个意思，我们将互换使用它们：**Image, Docker image, container image, and OCI image**.

一个 image 是一个只读包，包含了你需要运行 app 的任何事物。这意味着，他们包含了 app 代码、依赖、一个最小的操作系统结构集和原数据。你可以启动多个容器从一个单独 image。

获取 image 最简单的方式是从 registry 拉取。[Docker Hub](https://hub.docker.com) 是最通用的 registry，拉取一个 image 到你本地，Docker 可以使用它启动一个或多个容器。其他 registries 也存在，Docker 可以和他们一起工作。

image 是通过堆叠独立的层并将他们并将它们表示为单个统一的对象而生成的。一层可能是 OS 组件，另一层可能有 app 依赖，另外一层可能有 app。Docker 将这些层堆叠起来，使它们看起来像一个统一的系统。

Image 通常很小。例如，官方的 NGINX image 大约是60MB，官方的 Redis image 大约是40MB。但是，Windows映像可能非常大。



### 介绍 images

![](img/1735854570290.jpg)

一旦 container 运行，image 和 container 绑定，你不能删除 image直到停止和删除容器。



### Pulling images

image cashe：Linux 通常存储在 - **/var/lib/docker/<storage-driver>**

~~~shell
$ docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
nginx        latest    5e0fa356e6f4   5 weeks ago   197MB
ubuntu       latest    20377134ad88   6 weeks ago   101MB
~~~

获取 images 的过程叫做 pulling。

~~~shell
$ docker pull redis
Using default tag: latest
latest: Pulling from library/redis
f5c6876bb3d7: Already exists 
09fc6d52d57a: Pull complete 
00b1f2459268: Pull complete 
331d356ef102: Pull complete 
79f4acbc802f: Pull complete 
9b38153decf0: Pull complete 
4f4fb700ef54: Pull complete 
c46660a035a6: Pull complete 
Digest: sha256:bb142a9c18ac18a16713c1491d779697b4e107c22a97266616099d288237ef47
Status: Downloaded newer image for redis:latest
docker.io/library/redis:latest

$ docker images
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
redis        latest    c8ac0a9bed1d   3 months ago   140MB
~~~

Docker 固执己见，在拉取 images 时做了两个假设：

- 它假定您想要提取标记为最新的 images
- 它假定您想从 Docker Hub 提取 images



### Image registries

我们存储 image 到中心化的地方叫做 **registries**。

![](img/1735856117843.jpg)

一个 registries 包含一个或多个 image repositories，image repositories 包含一个或多个 images。

![](img/1735856337385.jpg)

#### Official repositories

下面的列表显示了一些官方存储库及其url，它们存在于 Docker Hub 命名空间的顶层：

- **nginx:** https://hub.docker.com/_/nginx/
- **busybox:** https://hub.docker.com/_/busybox/
- **redis:** https://hub.docker.com/_/redis/
- **mongo:** https://hub.docker.com/_/mongo/

![](img/1735856632189.jpg)



#### Unofficial repositories



### Image naming and tagging

下图显示了一个完全限定的映像名称，包括注册中心名称、用户/组织名称、存储库名称和标记。

![](img/1735865882655.jpg)

您只需要提供存储库名称和映像名称，两者之间用冒号分隔。有时我们称 image 名字是 tag。从官方存储库中提取 image 的格式 docker pull

```shell
$ docker pull <repository>:<tag>
```

 前面的示例使用以下命令提取 Redis 映像。它从顶级 redis 存储库中提取了标记为最新的图像。

~~~shell
$ docker pull redis:latest
~~~

下面的例子展示了如何拉出几个不同的官方图片。

~~~shell
$ docker pull mongo:7.0.5
//Pulls the image tagged as '7.0.5' from the official 'mongo' repository.
$ docker pull busybox:glibc
//Pulls the image tagged as 'glibc' from the official 'busybox' repository.
$ docker pull alpine
//Pulls the image tagged as 'latest' from the official 'alpine' repository.
~~~

有几件事情值得注意：

- 如果你没有一个具体的 image tag，docker 假设你想要 image tag 的 latest。如果 repository 没有 image tag latest，则命令会失败。
- 标记为最新的映像不能保证是存储库中最新的映像。

从非官方存储库中提取镜像与从官方存储库中提取镜像几乎相同——你只需要在存储库名称之前添加Docker Hub用户名或组织名称。下面的示例展示了如何从一个 Docker Hub ID 为 nigelpoulton 的不受信任的人拥有的 tu-demo 存储库中提取 v2 映像。

~~~shell
$ docker pull nigelpoulton/tu-demo:v2
~~~

要从不同的注册中心提取映像，只需在存储库名称之前添加注册中心的 DNS 名称。例如，下面的命令从 GitHub 容器注册表（ghcr.io）上的 Brandon Mitchell 的 regclient/regsync repo中提取最新的映像。

~~~shell
$ docker pull ghcr.io/regclient/regsync:latest
latest: Pulling from regclient/regsync
6f14f2b64ccf: Download complete
7746d6728537: Download complete
685af2c79c31: Download complete
4c377311167a: Download complete
662e9541e042: Download complete
Digest: sha256:149a95d47d6beed2a1404d7c3b00dddfa583a94836587ba8e3b4fe59853c1ece
Status: Downloaded newer image for ghcr.io/regclient/regsync:latest
ghcr.io/regclient/regsync:latest
~~~



#### Images with multiple tags

乍一看，下面的输出看起来像是列出了三个图像。然而，仔细观察，它只有两个—— b4210d0aa52f 图像被标记为最新和v1。

~~~shell
$ docker images
REPOSITORY               TAG		IMAGE ID			CREATED					SIZE
nigelpoulton/tu-demo     latest	b4210d0aa52f	2 days ago			115MB
nigelpoulton/tu-demo     v1			b4210d0aa52f	2 days ago			115MB
nigelpoulton/tu-demo     v2			6ba12825d092	12 minutes ago	115MB
~~~

这是一个很好的例子，最新的标签与最新的 image 无关。在本例中，最新标记引用与v1标记相同的图像，v1标记实际上比v2图像更老。



### Images and layers

![](img/1736115442544.jpg)

你将看到以下所有检查图层信息的方法：

- Pull operations
- `docker inspect` 命令
- `docker history` 命令

运行以下命令拉取最新的节点映像，并观察它拉取各个层。一些较新的版本可能有或多或少的层，但原理是相同的。

~~~shell
$ docker pull node:latest
latest: Pulling from library/ubuntu
952132ac251a: Pull complete
82659f8f1b76: Pull complete
c19118ca682d: Pull complete
8296858250fe: Pull complete
24e0251a0e2c: Pull complete
Digest: sha256:f4691c96e6bbaa99d...28ae95a60369c506dd6e6f6ab
Status: Downloaded newer image for node:latest
docker.io/node:latest
~~~

每一行 Pull complete 表示一层。这一个 images 有 5 层，下图展示了他每层的 id。

![](img/1736115793421.jpg)

下面实力检查相同的 node: latest 已被拉取的 image，在之前的步骤

~~~shell
$ docker inspect node:latest
[
    {
        "Id": "sha256:bd3d4369ae.......fa2645f5699037d7d8c6b415a10",
        "RepoTags": [
            "node:latest"
        <Snip>
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:c8a75145fc...894129005e461a43875a094b93412",
                "sha256:c6f2b330b6...7214ed6aac305dd03f70b95cdc610",
                "sha256:055757a193...3a9565d78962c7f368d5ac5984998",
                "sha256:4837348061...12695f548406ea77feb5074e195e3",
                "sha256:0cad5e07ba...4bae4cfc66b376265e16c32a0aae9"
											] 
								}
		} 
]
~~~

修剪后的输出显示了五个层。但是，它显示了它们的 SHA256 哈希值，这与 docker pull 输出中的 short ids 不同。

`docker inspect` 命令是一个很好的获取 image 详细信息的命令。

`docker history` 命令是检查 image 和他的层级的数据，然而，这个命令展示的是 image 构建历史，不是一个最终 image 的层级列表。例如，一些 Dockerfile 指令（ENV， EXPOSE， CMD 和 ENTRYPOINT）只添加元数据而不创建层。



#### Base layers

所有 docker image 开始都是一个 base layer，并且每一次你添加新的内容，docker 就会添加一个新层。

这意味着官方的 Ubuntu 24:04 镜像将是这个应用程序的基础层。安装Python将添加第二层，你的应用程序源代码将添加第三层。

最终的图像将有三个图层，如下图所示。请记住，出于演示目的，这是一个过于简化的示例。

![](img/1736116483937.jpg)

重要的是要理解图像是所有层按照构建顺序堆叠的组合。下图显示了一个有两层的图像。每层有三个文件，这意味着图像有六个文件。

![](img/1736116618763.jpg)

下图所示，File 7 可以掩盖 File 5

![](img/1736116763677.jpg)

图下图显示了上图中的三层图像将如何显示在系统上——所有三层叠加并合并为一个统一的视图。

![](img/1736116912239.jpg)

#### Sharing image layers

`docker pull` 命令，当本地有共享层时，则会跳过，因为在本地已经有了。

~~~shell
$ docker pull redis:latest
latest: Pulling from library/redis
25d3892798f8: Download complete
e5d458cf0bea: Download complete
4f4fb700ef54: Already exists	<<---- This line
<Snip>
~~~

![](img/1736117137540.jpg)

### Pulling images by digest

您有一个名为 golftrack:1.5 的映像，它被生产环境中的许多容器所使用，它有一个严重的错误。创建一个包含修复的新版本。

到目前为止，一切都很好，但是你犯了一个错误。

将新图像推到与易受攻击图像的标签相同的存储库。这将覆盖原始映像，使您无法很好地了解哪些生产容器正在使用易受攻击的映像，哪些正在使用固定映像—两个映像具有相同的标记！

`image digests` 来营救。Docker 使用内容可寻址存储模型，其中每个图像都有一个加密的内容哈希，我们通常称之为摘要。

如果您已经按名称提取了一个映像，那么您可以通过运行带有 `--digest` 标志的 docker images 命令来查看它的摘要，如下所示。

~~~shell
$ docker images --digests alpine
REPOSITORY   TAG       DIGEST                       IMAGE ID       CREATED      SIZE
alpine       latest    sha256:c5b1261d...8e1ad6b    c5b1261d6d3e   2 weeks ago  11.8MB
~~~

如果你想在拉出图像之前找到它的摘要，你可以使用 `docker buildx imagetools` 命令。下面的示例检索 Docker Hub 上nigelpoulton/k8sbook/latest映像的映像摘要。

~~~shell
$ docker buildx imagetools inspect nigelpoulton/k8sbook:latest
Name:      docker.io/nigelpoulton/k8sbook:latest
MediaType: application/vnd.docker.distribution.manifest.list.v2+json
Digest:    sha256:13dd59a0c74e9a147800039b1ff4d61201375c008b96a29c5bd17244bce2e14b
<Snip>
~~~

现在可以使用摘要提取图像。

~~~shell
$ docker pull nigelpoulton/k8sbook@sha256:13dd59a0...bce2e14b
docker.io/nigelpoulton/k8sbook@sha256:13dd59a0...bce2e14b: Pulling from nigelpoulton/k8sbook
59f1664fb787: Download complete
a052f1888b3e: Download complete
94a9f4dfa0e5: Download complete
bb7e600677fa: Download complete
edfb0c26f1fb: Download complete
5b1423465504: Download complete
2f232a362cd9: Download complete
Digest: sha256:13dd59a0...bce2e14b
Status: Downloaded newer image for nigelpoulton/k8sbook@sha256:13dd59a0...bce2e14b
docker.io/nigelpoulton/k8sbook:latest@sha256:13dd59a0...bce2e14b
~~~

也可以直接查询注册表API获取图像数据，包括摘要。下面的 curl 命令向 Docker Hub 查询相同图像的摘要。

~~~shell
$ curl "https://hub.docker.com/v2/repositories/nigelpoulton/k8sbook/tags/?name=latest" \
|jq '.results[].digest'
"sha256:13dd59a0c74e9a147800039b\
1ff4d61201375c008b96a29c5bd17244bce2e14b"
~~~



#### Image hashes and layer hashes

images 和 layers 有他们自己的 digests 如下：

- Images digests 是 image 清单文件的加密散列
- Layer digests 是层的内容的加密散列

这意味着对图层或图像清单的所有更改都会产生新的哈希值，从而为我们提供了一种简单可靠的方式来了解是否进行了更改。



#### Content hashes vs distribution hashes

Docker在每次推拉之前和之后比较哈希值，以确保没有发生篡改。

但是，它还在推和拉操作期间压缩映像，以节省网络带宽和注册表上的存储空间。这种压缩的结果是，前后散列不匹配。

为了解决这个问题，每层都有两个哈希值：

- Content hash (uncompressed)
- Distribution hash (compressed)

每次 Docker 从注册表中推送或提取一个层时，它都会包含该层的分布哈希值，并使用它来验证是否发生了逐渐减少。这就是为什么不同CLI 和注册表输出中的哈希值并不总是匹配的原因之一——有时您查看的是内容哈希值，而有时您查看的是分布哈希值。



### Multi-architecture images

Docker 和 registry API 进行了调整，变得足够聪明，可以在单个标签后面隐藏多个架构的图像。

这意味着你可以使用 `docker pull alpine` 在任意架构并且得到正确的 image 版本。例如，如果你在 AMD64 机器上，你讲得到 AMD64 image。

为了实现这一点，Registry API支持两个重要的构造：

- Manifest lists
- Manifests

清单列表顾名思义——一个图像标记支持的体系结构列表。然后，每个受支持的体系结构都有自己的清单，其中列出了用于构建它的层。

运行如下命令可以看到不同架构支撑在 **alpine:latest** tag：

~~~shell
$ docker buildx imagetools inspect alpine
Name:      docker.io/library/alpine:latest
MediaType: application/vnd.docker.distribution.manifest.list.v2+json
Digest:    sha256:c5b1261d6d3e43071626931fc004f70149baeba2c8ec672bd4f27761f8e1ad6b
Manifests:
  Name:      docker.io/library/alpine:latest@sha256:6457d53f...628977d0
  MediaType: application/vnd.docker.distribution.manifest.v2+json
  Platform:  linux/amd64
  
  Name:      docker.io/library/alpine:latest@sha256:b229a851...d144c1d8
  MediaType: application/vnd.docker.distribution.manifest.v2+json
  Platform:  linux/arm/v6
  
  Name:      docker.io/library/alpine:latest@sha256:ec299a7b...33b4c6fe
  MediaType: application/vnd.docker.distribution.manifest.v2+json
  Platform:  linux/arm/v7
  
  Name:      docker.io/library/alpine:latest@sha256:a0264d60...93467a46
  MediaType: application/vnd.docker.distribution.manifest.v2+json
  Platform:  linux/arm64/v8
  
  Name:      docker.io/library/alpine:latest@sha256:15c46ced...ab073171
  MediaType: application/vnd.docker.distribution.manifest.v2+json
  Platform:  linux/386
  
  Name:      docker.io/library/alpine:latest@sha256:b12b826d...ba52a3a2
  MediaType: application/vnd.docker.distribution.manifest.v2+json
  Platform:  linux/ppc64le
~~~

如果你仔细观察，你会看到一个单清单指向六个清单。

**MediaType: application/vnd.docker.distribution.manifest.list.v2+json** 是 Manifest lists

每一个 **MediaType: application/vnd.docker.distribution.manifest.v2+json** 行指的是每个 manifest 具体架构。

![](img/1736289668561.jpg)

让我们通过一个快速事例逐步了解。

假设你在 M3 Mac 上使用 Docker 桌面，Docker 在 linux/arm 虚拟机中运行。你要求 Docker 提取一个映像，Docker 对注册表API进行相应的调用，请求相应的清单列表。假设它存在，Docker 然后将其解析为 linux/arm 条目。如果 linux/arm 条目存在，Docker 检索它的清单，解析它的层的加密id，拉出每个单独的层，并将它们组装到映像中。

Linux on arm64 example:

```shell
$ docker run --rm golang go version
<Snip>
go version go1.22.0 linux/arm64
```

Windows on x64 example:

```shell
> docker run --rm golang go version
<Snip>
go version go1.20.4 windows/amd64
```



~~~shell
$ docker manifest inspect golang | grep 'architecture\|os'
            "architecture": "amd64",
            "os": "linux"
            "architecture": "arm",
            "os": "linux",
            "architecture": "arm64",
            "os": "linux",
            "architecture": "386",
            "os": "linux"
            "architecture": "mips64le",
            "os": "linux"
            "architecture": "ppc64le",
            "os": "linux"
            "architecture": "s390x",
            "os": "linux"
            "architecture": "amd64",
            "os": "windows",
            "os.version": "10.0.20348.2227"
            "architecture": "amd64",
            "os": "windows",
            "os.version": "10.0.17763.5329"
~~~



`docker buildx` 命令是他简单的去创建多架构 images。`docker buildx` 提供两种方式创建 multi-architectureimages：

- Emulation
- Build Cloud

使用 Docker build Cloud 运行以下命令来构建 AMD 和 ARM 版本的 nigelpoulton/tu-demo 映像。

~~~shell
$ docker buildx build \
  --builder=cloud-nigelpoulton-ddd-cloud \
  --platform=linux/amd64,linux/arm64 \
  -t nigelpoulton/tu-demo:latest --push .
~~~



### 使用 Docker Scout 进行漏洞扫描

Docker Scout 是一项非常巧妙的服务，但它需要付费订阅。

你可以使用 `docker scout quickview` 命令的到一个 image 的漏洞概览。下面命令是分析 **nigelpoulton/tu- demo:latest** image。如果一个本地副本不存在，他将会从 Docker Hub 拉取并在本地执行分析。

![](img/1736290793293.jpg)

输出显示 0 关键漏洞 (0C), one high (1H), one medium (1M), and zero low (0L).



### Deleting Images

使用 `docker rmi` 命令删除 images。**rmi** 是 **remove image** 的缩写。

您可以通过名称、短ID 或 SHA 删除镜像。也可以使用同一命令删除多个镜像。

下面的命令删除三个映像——一个按名称，一个按短ID，一个按SHA。

~~~shell
$ docker rmi redis:latest af111729d35a sha256:c5b1261d...f8e1ad6b
Untagged: redis:latest
Deleted: sha256:76d5908f5e19fcdd73daf956a38826f790336ee4707d9028f32b24ad9ac72c08
Untagged: nigelpoulton/tu-demo:v2
Deleted: sha256:af111729d35a09fd24c25607ec045184bb8d76e37714dfc2d9e55d13b3ebbc67
Untagged: alpine:latest
Deleted: sha256:c5b1261d6d3e43071626931fc004f70149baeba2c8ec672bd4f27761f8e1ad6b
~~~

可以通过 `-f` 标记强制操作。

~~~shell
$ docker rmi $(docker images -q) -f
~~~

为了理解如何工作，下载几个 images，然后运行 `docker images -q`

~~~shell
$ docker pull alpine
<Snip>

$ docker pull ubuntu
<Snip>

$ docker images -q
44dd6f223004
3f5ef9003cef

$ docker rmi $(docker images -q) -f
Untagged: alpine:latest
Untagged: alpine@sha256:02bb6f428431fb...a33cb1af4444c9b11
Deleted: sha256:44dd6f2230041...09399391535c0c0183b
Deleted: sha256:94dd7d531fa56...97252ba88da87169c3f
Untagged: ubuntu:latest
Untagged: ubuntu@sha256:dfd64a3b4296d8...9ee20739e8eb54fbf
Deleted: sha256:3f5ef9003cefb...79cb530c29298550b92
Deleted: sha256:b49483f6a0e69...f3075564c10349774c3

$ docker images
REPOSITORY     TAG    IMAGE ID    CREATED     SIZE
~~~



### Images 命令

- `docker pull`：从远程 registries 拉取 images；`docker pull alpine:latest` 拉取 tag 是 latest 的 alpine repository。
- `docker images`：列出本地所有的 images。 `--digests` 标志看到 SHA256 hashes。
- `docker inspect`：在格式良好的视图中为您提供丰富的与图像相关的元数据。
- `docker manifest inspect`：允许您检查存储在注册表中的图像清单列表。以下命令将显示 regctl image on GitHub Container Registry（GHCR）的清单列表：docker manifest inspect GHCR .io/regclient/regctl。
- `docker buildx`：是一个Docker CLI 插件，使用 Docker 最新的构建引擎功能。您看到了如何使用 image tools 子命令从图像中查询与清单相关的数据。
- `docker scout`：是一个Docker CLI插件，它集成了Docker Scout后端来执行图像漏洞扫描。它扫描图像，提供漏洞报告，甚至建议补救措施。
- `docker rmi`：用于删除镜像。它删除存储在本地文件系统中的所有层数据，并且您不能删除与处于运行（Up）或停止（exit）状态的容器关联的映像。



## 第七章 Working with containers

Docker 实现了 the Open Container Initiative (OCI) specifications. 

### Containers – The TLDR

下图展示多个 containers  从一个单独的 image 启动。共享 image 是只读的，但是你可以写到容器中。

![](img/1736730123248.jpg)

您可以启动、停止、重启和删除容器，就像对虚拟机一样。

容器是被设计为无状态与短暂的。

容器也被设计为不可变的。这意味着在部署了容器之后不应该更改它们——如果容器失败了，可以用一个新容器替换它，而不是连接到它并在活动实例中进行修复。

 容器应该只运行一个进程，我们用它们来构建微服务应用。例如，一个具有web服务器、身份验证、目录和存储等四个特性（微服务）的应用程序将拥有四个容器——一个运行web服务器，一个运行身份验证服务，一个运行目录，另一个运行存储。



### Containers vs VMs

- VMs 虚拟化硬件
- Containers 虚拟化操作系统

下图展示的是容器特行更有效，运行了比 VMs 3倍多的 app。

![](img/1736806871536.jpg)



### The VM tax

虚拟机模式有一个最大的问题是你需要安装 OS 在每个 VM 上 —— 每个 OS 都需要消耗CPU、RAM和存储，并且需要较长的启动时间。

容器通过在它们运行的主机上共享一个操作系统来解决所有这些问题。

这使得容器比虚拟机具有以下所有优点：

- 容器更小、更便捷
- 您可以在基础设施上运行更多容器
- 容器启动更快
- 容器减少了需要管理的操作系统的数量（补丁、更新等）。
- 容器的攻击面较小



### Images and Containers

如下图所示，Docker 通过为每个容器创建一个薄读写层并将其放在共享映像的顶部来实现这一点。

![](img/1736807796705.jpg)

### Check Docker is running

`docker version` 检查 Docker 是否启动。这是一个很好的命令，因为它检查CLI和引擎组件。

~~~shell
$ docker version
Client:
 Version:           27.3.1
 API version:       1.47
 Go version:        go1.22.7
 Git commit:        ce12230
 Built:             Fri Sep 20 11:38:18 2024
 OS/Arch:           darwin/arm64
 Context:           desktop-linux

Server: Docker Desktop 4.36.0 (175267)
 Engine:
  Version:          27.3.1
  API version:      1.47 (minimum version 1.24)
  Go version:       go1.22.7
  Git commit:       41ca978
  Built:            Fri Sep 20 11:41:19 2024
  OS/Arch:          linux/arm64
  Experimental:     false
 containerd:
  Version:          1.7.21
  GitCommit:        472731909fa34bd7bc9c087e4c27943f9835f111
 runc:
  Version:          1.1.13
  GitCommit:        v1.1.13-0-g58aa920
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
~~~



### Starting a container

`docker run` 命令是启动新容器的最简单、最常见的方法。

运行以下命令启动一个名为 webserver 的新容器。

~~~shell
$ docker run -d --name webserver -p 5005:8080 nigelpoulton/ddd-book:web0.1
Unable to find image 'nigelpoulton/ddd-book:web0.1' locally
web0.1: Pulling from nigelpoulton/ddd-book
4f4fb700ef54: Already exists
cf2a607f33f7: Download complete
0a1f0c111e9a: Download complete
c1af4b5db242: Download complete
Digest: sha256:3f5b281b914b1e39df8a1fbc189270a5672ff9e98bfac03193b42d1c02c43ef0
Status: Downloaded newer image for nigelpoulton/ddd-book:web0.1
b5594b3b8b3fdce544d2ca048e4340d176bce9f5dc430812a20f1852c395e96b
~~~

`docker run` 告诉 docker 去运行一个新的 容器。

`-d` 标志告诉 Docker 在后台将其作为守护进程运行，并与本地终端分离

`name` 标志告诉 Docker 将这个容器命名为 **webserver**。

`-p 5005:8080` 标志映射端口 5005 到容器端口 8080。这是因为容器的web服务器正在监听端口8080。

**nigelpoulton/ddd-book:web0.1** 参数是告诉 Docker 使用哪一个 image 来启动 container。

当你点击Return时，Docker客户端将命令转换为API请求，并将其发送到Docker守护进程公开的Docker API。

运行以下命令来验证镜像是否在本地和新容器中被拉出被称为 web 服务器正在运行。

~~~shell
$ docker images

$ docker ps

~~~



### How containers start apps

有三种方式启动一个 app 在一个容器中：

- image 中的入口点指令
- image 中的 cmd 指令
- 一个 CLI 参数



~~~shell
$ docker inspect nginx | grep Entrypoint -A 3
            "Entrypoint": [
                "/docker-entrypoint.sh"
            ],
            "OnBuild": null,
~~~



~~~shell
$ docker run <arguments> <image> <command>
~~~

如果你具体指出一个 command，他将会重写 Cmd 执行，但将会附加到入口点执行中。



### Connecting to a container

可以使用 `docker exec` 命令到运行中的 container 执行命令，并且他有两个模式：

- Interactive
- Remote execution

交互式exec会话将终端连接到容器中的shell进程，其行为类似于远程SSH会话。

远程执行模式允许您向正在运行的容器发送命令，并将输出输出到本地终端。



`-it` 标志使其成为交互式执行会话，sh 参数在容器内启动一个新的 sh 进程。sh 是安装在容器中的最小 shell 程序。

~~~shell
$ docker exec -it webserver sh
/src #
~~~



### Inspecting container processes

大多数容器只运行单个进程。这是容器的主应用进程，并且总是PID 1。

运行 **ps** 命令查看容器中运行的进程。

~~~shell
/src # ps
PID   USER	TIME  COMMAND
    1 root	0:00 node ./app.js
   13 root	0:00 sh
   22 root	 0:00 ps
~~~

输出显示了三个进程：

- PID 1 是运行 Node.js web应用程序的主要应用进程
- PID 13 是你的交互式 exec 会话连接到的shell进程
- PID 22 是您刚刚运行的 ps 命令



### **The** docker inspect command

~~~shell
$ docker inspect ctr1
[
    {
        "Id": "e06d47ef5637e5befcb9df5f5545e129570060aa2e85dde2f016c941e0f83352",
        "Created": "2025-01-16T22:55:50.232295834Z",
        "Path": "/docker-entrypoint.sh",
        "Args": [
            "nginx",
            "-g",
            "daemon off;"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 638,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2025-01-16T22:55:50.263238834Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:5e0fa356e6f4a996ca452b017c81aca3d087ae38f873ed8314af16126423b21f",
        "ResolvConfPath": "/var/lib/docker/containers/e06d47ef5637e5befcb9df5f5545e129570060aa2e85dde2f016c941e0f83352/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/e06d47ef5637e5befcb9df5f5545e129570060aa2e85dde2f016c941e0f83352/hostname",
        "HostsPath": "/var/lib/docker/containers/e06d47ef5637e5befcb9df5f5545e129570060aa2e85dde2f016c941e0f83352/hosts",
        "LogPath": "/var/lib/docker/containers/e06d47ef5637e5befcb9df5f5545e129570060aa2e85dde2f016c941e0f83352/e06d47ef5637e5befcb9df5f5545e129570060aa2e85dde2f016c941e0f83352-json.log",
        "Name": "/ctr1",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "bridge",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "ConsoleSize": [
                27,
                120
            ],
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "private",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": [],
            "BlkioDeviceWriteBps": [],
            "BlkioDeviceReadIOps": [],
            "BlkioDeviceWriteIOps": [],
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": null,
            "PidsLimit": null,
            "Ulimits": [],
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware",
                "/sys/devices/virtual/powercap"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/8c575cedfe65d636e39d44a0b33bceaea9a78e3c05840e989e84efc64c753cfe-init/diff:/var/lib/docker/overlay2/c1bf20dec09f909c809cf3692f0ae523dcb26710a13c2809789ac572ed2739d5/diff:/var/lib/docker/overlay2/cd79e64a1b5576c3c72af85b77d259031337f345eea2092c4887327aa2adbc38/diff:/var/lib/docker/overlay2/8eb37748ceb8faacef1bb05c96ca1ff85f61840f3878eb8820cf1a2092e8a8c7/diff:/var/lib/docker/overlay2/6aad85f70fc2bd33590bd13c6a39b3c7dae0f4dde797f1bc4eb3c8deb9b6e8c6/diff:/var/lib/docker/overlay2/862f7ee2ec2d72d4c99827c2b7e769b801d33c89ae3ca7cd5ec33a1d9ddc6f5d/diff:/var/lib/docker/overlay2/ce591b2c753b77867fae6b492cfb77371e235f15fed93a321239a8a5d81bf45c/diff:/var/lib/docker/overlay2/c4858e1c5d68d63cf0a625078c00ba0625cff3830a70c3693594ded311a9f216/diff",
                "MergedDir": "/var/lib/docker/overlay2/8c575cedfe65d636e39d44a0b33bceaea9a78e3c05840e989e84efc64c753cfe/merged",
                "UpperDir": "/var/lib/docker/overlay2/8c575cedfe65d636e39d44a0b33bceaea9a78e3c05840e989e84efc64c753cfe/diff",
                "WorkDir": "/var/lib/docker/overlay2/8c575cedfe65d636e39d44a0b33bceaea9a78e3c05840e989e84efc64c753cfe/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "e06d47ef5637",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "80/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "NGINX_VERSION=1.27.3",
                "NJS_VERSION=0.8.7",
                "NJS_RELEASE=1~bookworm",
                "PKG_RELEASE=1~bookworm",
                "DYNPKG_RELEASE=1~bookworm"
            ],
            "Cmd": [
                "nginx",
                "-g",
                "daemon off;"
            ],
            "Image": "nginx",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": [
                "/docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {
                "maintainer": "NGINX Docker Maintainers \u003cdocker-maint@nginx.com\u003e"
            },
            "StopSignal": "SIGQUIT"
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "17cebadf428eed42856be803166ddc876442da8efc0d2fcb1154cc3d0ddd4209",
            "SandboxKey": "/var/run/docker/netns/17cebadf428e",
            "Ports": {
                "80/tcp": null
            },
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "5d70e620e745666a9bc344cba9ba057e66176a07fe54cbc07b18124c644ff4a5",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null,
                    "NetworkID": "e4fffbf009e14dd23212b99172b0aeb0f5a9dfff036494f9c8209f9a29b83233",
                    "EndpointID": "5d70e620e745666a9bc344cba9ba057e66176a07fe54cbc07b18124c644ff4a5",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "DNSNames": null
                }
            }
        }
    }
]
~~~

建议花一些时间研究一下 `docker inspect` 命令的输出。



### Stopping, restarting, and deleting a container

~~~shell
$ docker ps
CONTAINER ID   IMAGE						COMMAND					STATUS			PORTS							NAMES
b5594b3b8b3f   nigelpoulton...	"node ./app.js"	Up 51 mins	0.0.0.0:80->8080	webserver
~~~

`docker stop` 命令停止。它需要10秒才能正常停止。

~~~shell
$ docker stop webserver
webserver
~~~

运行 `docker ps` 命令

~~~shell
$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
~~~

运行 `docker ps -a` 查询所有的容器，包括停止的容器。

~~~shell
$ docker ps -a
CONTAINER ID   IMAGE         COMMAND           STATUS                           NAMES
b5594b3b8b3f   nigelpou...   "node ./app.js"   Exited (137) About a minute ago  webserver
~~~

正如您在输出中看到的，它仍然存在，但处于退出状态。使用以下命令重新启动它。

~~~shell
$ docker restart webserver
webserver
~~~

强制操作

~~~shell
$ docker rm webserver -f
webserver
~~~



### Killing a container’s main process

运行以下命令，基于 Ubuntu 映像启动一个名为 **ddd-ctr** 的新交互式容器，并告诉它运行 Bash shell 作为其主进程。

~~~shell
$ docker run --name ddd-ctr -it ubuntu:24.04 bash
Unable to find image 'ubuntu:24.04' locally
24.04: Pulling from library/ubuntu
51ae9e2de052: Download complete
Digest: sha256:ff0b5139e774bb0dee9ca8b572b4d69eaec2795deb8dc47c8c829becd67de41e
Status: Downloaded newer image for ubuntu:24.04
root@d3c892ad0eb3:/#
~~~

运行 ps 命令，列出所有运行的进程

~~~shell
root@d3c892ad0eb3:/# ps
PID 	TTY     TIME 		 CMD
1 		pts/0		00:00:00 bash
9 		pts/0		00:00:00 ps
~~~

输入exit返回到本地终端，然后运行docker ps -a命令，看看容器是否终止。

~~~shell
$ docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS                     PORTS     NAMES
155a2a6c1648   ubuntu:24.04   "bash"                   6 minutes ago   Exited (0) 6 seconds ago             ddd-ctr
~~~

您可以键入**Ctrl PQ**退出容器，而不会终止所连接的进程。



### Debugging slim images and containers with Docker Debug

Debug 是一个新的工具，需要订阅，也就是说不是免费。



### Self-healing containers with restart policies

> 带有重启策略的自修复容器

你可以为每个容器应用重启策略，Docker支持以下四种策略：

- **no** (default)
- **on-failure**
- **always**
- **unless-stopped**

下图展示不同策略的响应

![](img/1737326977709.jpg)

让我们通过使用 `--restart always` 标志启动一个新的交互式容器并告诉它运行shell进程来演示always策略。

~~~shell
$ docker run --name neversaydie -it --restart always alpine sh
/#
~~~

键入 exit 终止 shell 进程并返回到本地终端。这将导致容器以 **zero exit code** 退出，表示正常退出，没有任何失败。根据上表，always restart 策略应该自动重新启动容器。

~~~shell
$ docker ps   
CONTAINER ID   IMAGE     COMMAND   CREATED              STATUS         PORTS     NAMES
fedb9b6a3820   alpine    "sh"      About a minute ago   Up 4 seconds             neversaydie
~~~

容器按预期运行。但是，您可以看到它是在15秒前创建的，但只运行了2秒。这是因为在终止shell进程时强制它退出，然后Docker自动重新启动它。同样重要的是要知道Docker重新启动了同一个容器，而不是创建一个新的容器。事实上，如果您的应用程序检查它，您将看到therestartcounts已被增加为1。记住用Select- String - pattern ‘RestartCount’代替grep，如果你是onwindowsusingpowershell。

~~~shell
$ docker inspect neversaydie | grep RestartCount
        "RestartCount": 1,
~~~

`--restart always` 策略的一个有趣特性是，如果您使用docker stop停止容器，然后重新启动docker守护进程，docker将在守护进程启动时重新启动容器。需要明确的是：

~~~shell
services:
  myservice:
    <Snip>
    restart_policy:
      condition: always | unless-stopped | on-failure
~~~

1. You start a new container with the **--restart always** policy 
2. You manually stop it with the **docker stop** command

3. You restart Docker (or an event causes Docker to restart)
4. When Docker comes back up, it starts the *stopped container*



#### Clean up

**docker rm <container> -f**：删除独立容器

**docker rmi**：删除 images

`docker rm $(docker ps -aq) -f`：删除所有容器

`docker rmi $(docker images -q)`：删除所有 images



### Containers 命令

- `docker run`：**docker run -it ubuntu bash**.
- **Ctrl-PQ** 退出时，不会杀掉容器进程
- **docker ps** 列出所有运行容器 **-a** 参数也可以查看已经退出的容器
- **docker exec** 在容器中运行命令 **docker exec -it <container-name> bash**.
- **docker stop ** 停止容器
- **docker restart** 重启一个停止的容器
- **docker rm** 删除一个停止的容器。 **-f** 可以删除运行中的容器
- **docker inspect** 展示容器容器的详细配置与运行时信息
- **docker debug** 收费



## 第八章 容器化一个应用

Docker 可以轻松的打包一个一个应用成 images，并且运行他通过容器。这个过程为：**容器化**。本章将引导您完成整个过程。

我将这一章划分如下：

- 容器化一个应用
- 容器化一个单独容器 APP
- 通过多阶段构建转移到生产环境
- Buildx, BuildKit, drivers, and Build Cloud
- Multi-architecture builds
- 几个好的实践

### 容器化一个 App

Docker 目的是简单的构建、转移并运行应用。这个过程看起来像下面所示：

1. 编写应用程序并创建依赖项列表
2. 创建一个 Dokcerfile 告诉 Docker 如何构建并运行 app
3. 构建 app 到 image
4. 推送 image 到 registry（可选）
5. 根据 image 运行容器

如下图所示：

<img src="img/1737399813131.jpg" style="zoom:50%;" />

### 容器化一个单独容器 app

在本节中，你将完成以下步骤来容器化一个简单的Node.js应用：

- Get the application code from GitHub
- Create the Dockerfile
- Containerize the app
- Run the app
- Test the app
- Look a bit closer - 仔细看一下



#### 获取应用代码

> 本示例会根据 spring boot 服务，打包一个 app。

根据 start.spring.io 创建新的服务。

在服务的目录就是你构建的内容，因为他包含了应用源码和文件列表依赖。



#### 创建 Dockerfile

在过去，您必须手动创建Dockerfiles。幸运的是，新版本的Docker支持Docker init命令，该命令可以分析应用程序并自动创建实现良好实践的dockerfile。

运行 `docker init` 插件

~~~shell
$ docker init

Welcome to the Docker Init CLI!

This utility will walk you through creating the following files with sensible defaults for your project:
  - .dockerignore
  - Dockerfile
  - compose.yaml
  - README.Docker.md

Let's get started!

? What application platform does your project use? Other

✔ Created → .dockerignore
✔ Created → Dockerfile
✔ Created → compose.yaml
✔ Created → README.Docker.md

→ Your Docker files are ready!
  Review your Docker files and tailor them to your application.
  Consult README.Docker.md for information about using the generated files.

What's next?
  Start your application by running → docker compose up --build
AMAR64XLRH7PP:build-docker-image-sample h.a.hu$ 
~~~



#### 容器化应用

~~~shell
$ docker build -t hello-world:wolfman.app .
[+] Building 5.8s (12/12) FINISHED                                                                                                                     docker:desktop-linux
 => [internal] load build definition from Dockerfile                                                                                                                   0.0s
 => => transferring dockerfile: 2.91kB                                                                                                                                 0.0s
 => resolve image config for docker-image://docker.io/docker/dockerfile:1                                                                                              3.7s
 => docker-image://docker.io/docker/dockerfile:1@sha256:93bfd3b68c109427185cd78b4779fc82b484b0b7618e36d0f104d4d801e66d25                                               1.7s
 => => resolve docker.io/docker/dockerfile:1@sha256:93bfd3b68c109427185cd78b4779fc82b484b0b7618e36d0f104d4d801e66d25                                                   0.0s
 => => sha256:93bfd3b68c109427185cd78b4779fc82b484b0b7618e36d0f104d4d801e66d25 8.40kB / 8.40kB                                                                         0.0s
 => => sha256:0e0e6acfbaab9caf25816a99db624e527adf2af3a84b6f66f24c520bc770a5ac 850B / 850B                                                                             0.0s
 => => sha256:3b04eb14fbd0bc0d9dd71f3859c4b682e0e39f132f18d858e12c7ecdf71125d3 1.26kB / 1.26kB                                                                         0.0s
 => => sha256:53c1bb3ab8a262a769465d960a53bf02d2fd4fbfb4cdd6c7d591b44f4e285f19 12.01MB / 12.01MB                                                                       1.5s
 => => extracting sha256:53c1bb3ab8a262a769465d960a53bf02d2fd4fbfb4cdd6c7d591b44f4e285f19                                                                              0.1s
 => [internal] load build definition from Dockerfile                                                                                                                   0.0s
 => WARN: FromAsCasing: 'as' and 'FROM' keywords' casing do not match (line 19)                                                                                        0.0s
 => WARN: FromAsCasing: 'as' and 'FROM' keywords' casing do not match (line 29)                                                                                        0.0s
 => [internal] load metadata for docker.io/library/alpine:latest                                                                                                       0.0s
 => [internal] load .dockerignore                                                                                                                                      0.0s
 => => transferring context: 708B                                                                                                                                      0.0s
 => [base 1/1] FROM docker.io/library/alpine:latest                                                                                                                    0.0s
 => [build 1/2] RUN echo -e '#!/bin/sh\necho Hello world from $(whoami)! In order to get your application running in a container, take a look at the comments in the   0.1s
 => [final 1/2] RUN adduser     --disabled-password     --gecos ""     --home "/nonexistent"     --shell "/sbin/nologin"     --no-create-home     --uid "10001"     a  0.2s
 => [build 2/2] RUN chmod +x /bin/hello.sh                                                                                                                             0.1s
 => [final 2/2] COPY --from=build /bin/hello.sh /bin/                                                                                                                  0.0s
 => exporting to image                                                                                                                                                 0.0s
 => => exporting layers                                                                                                                                                0.0s
 => => writing image sha256:417a87bbd3b339ccf9dfdfb2faa3dbc3644a24478fbcd9b0b2a5145871cfe6a3                                                                           0.0s
 => => naming to docker.io/library/hello-world:wolfman.app                                                                                                             0.0s

 2 warnings found (use docker --debug to expand):
 - FromAsCasing: 'as' and 'FROM' keywords' casing do not match (line 19)
 - FromAsCasing: 'as' and 'FROM' keywords' casing do not match (line 29)
~~~



#### Push the image to Docker Hub 可选



#### 运行应用

~~~shell
$ docker run -d --name helloWorld -p 8080:8080 hello-world:wolfman.app
c9f7be93b522303e26cdbb136512bfe33779dba84c05b8381b4733cf689ae8cd

$ docker ps 
CONTAINER ID   IMAGE     COMMAND   CREATED        STATUS             PORTS     NAMES
fedb9b6a3820   alpine    "sh"      21 hours ago   Up About an hour             neversaydie
~~~



#### 测试 APP



#### 再仔细看一下

现在您已经将应用程序容器化了，让我们来仔细看看其中的一些机制是如何工作的。

**docker build** 命令从顶部开始，一行一行地解析Dockerfile。您可以通过以#字符开始的行插入注释，构建器将忽略它们。

所有非注释行都称为**指令**或**步骤**，格式为 <INSTRUCTION> <arguments>。指令名不区分大小写，但为了更容易阅读，通常将它们写成大写。

一些指令创建新的层，而其他指令添加元数据。

创建新层的指令示例有 FROM， RUN， COPY 和 WORKDIR。创建元数据的例子包括EXPOSE、ENV、CMD和ENTRYPOINT。前提是：

- 添加内容（如文件和程序）、创建新层的说明
- 不添加内容的指令不添加层，只创建元数据

您可以对任何 image 运行 docker history 命令，以查看创建它的指令。

~~~shell
% docker history hello-world:latest
IMAGE          CREATED        CREATED BY                                      SIZE      COMMENT
11285982a72f   10 hours ago   ENTRYPOINT ["java" "-jar" "build-docker-imag…   0B        buildkit.dockerfile.v0
<missing>      10 hours ago   EXPOSE map[8080/tcp:{}]                         0B        buildkit.dockerfile.v0
<missing>      10 hours ago   COPY build/libs/build-docker-image-sample-0.…   20.7MB    buildkit.dockerfile.v0
<missing>      10 hours ago   WORKDIR /app                                    0B        buildkit.dockerfile.v0
<missing>      3 years ago    /bin/sh -c #(nop)  CMD ["jshell"]               0B        
<missing>      3 years ago    /bin/sh -c set -eux;   arch="$(apk --print-a…   318MB     
<missing>      3 years ago    /bin/sh -c #(nop)  ENV JAVA_VERSION=17-ea+14    0B        
<missing>      3 years ago    /bin/sh -c #(nop)  ENV PATH=/opt/openjdk-17/…   0B        
<missing>      3 years ago    /bin/sh -c #(nop)  ENV JAVA_HOME=/opt/openjd…   0B        
<missing>      3 years ago    /bin/sh -c apk add --no-cache java-cacerts      2.38MB    
<missing>      3 years ago    /bin/sh -c #(nop)  CMD ["/bin/sh"]              0B        
<missing>      3 years ago    /bin/sh -c #(nop) ADD file:f278386b0cef68136…   5.6MB     
~~~

从输出中有几点值得注意。

所有以 buildkit.dockerfile.v0 相关的指令行都是 dockerfile 用于构建 image 得指令。

**CREATED BY** 列列出了准确的  dockerfile 指令是创建 layer 或者是原数据。

在 **SIZE** 列中具有非零值的行创建新层，而具有0B的行仅添加元数据。

运行 **docker inspect** 查看图像层列表。

~~~shell
$ docker inspect hello-world:latest
<Snip>
},
"RootFS": {
    "Type": "layers",
    "Layers": [
        "sha256:72e830a4dff5f0d5225cdc0a320e85ab1ce06ea5673acfe8d83a7645cbd0e9cf",
        "sha256:5836ece05bfd8a5425efcf10075dfd206079fc6c10880dabbc38dd9701ab44bf",
        "sha256:34f7184834b2dbb736d5d07c9835e4ca13289b9a90afb40780f7c8a68f2f6ccb",
        "sha256:d7fb0a03fec96eab35cd62cf99216e87935b559716f772ae2728f7dc2e6bf2c9",
        "sha256:c4bb876c5112a5f189df5bb25cbbe8b4291a5ba470831d244bb519b7dfa51bb8"
    ]
},
~~~

通常认为使用 Docker 官方映像和经过验证的 Publisher 映像作为创建新映像的基础层是一种很好的做法。

**这是因为他们保持高标准并快速实现已知漏洞的修复。**



### 











































