# Docker-Learning

### 什么是docker

当人们说“Docker”时，他们通常是指 Docker Engine，它是一个客户端 - 服务器应用程序，由 Docker

守护进程、一个REST API指定与守护进程交互的接口、和一个命令行接口（CLI）与守护进程通信（通过

封装REST API）。Docker Engine 从 CLI 中接受docker 命令，例如 docker run 、docker ps 来列出正

在运行的容器、docker images 来列出镜像，等等。

- docker是一个软件，可以运行在window、linux、mac等各种操作系统上。


- docker 是一个开源的应用容器引擎，基于Go 语言开发并遵从 Apache2.0 协议开源，项目代码托管在github上进行维护。


- docker 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上。


- 容器是完全使用沙箱机制，相互之间不会有任何接口,更重要的是容器性能开销极低。

### docker基本组成

- docker主机(Host)：安装了Docker程序的机器（Docker直接安装在操作系统之上）；


- docker仓库(Registry)：用来保存各种打包好的软件镜像；仓库分为公有仓库和私有仓库。(很类似maven) 


- docker镜像(Images)：软件打包好的镜像；放在docker仓库中；


- docker容器(Container)：镜像启动后的实例称为一个容器；容器是独立运行的一个或一组应用