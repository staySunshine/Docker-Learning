# Docker镜像常用命令

## pull命令

### 示例

`docker pull tomcat:9.0.20-jre8`

实际上相当于 `docker pull docker.io/tomcat:9.0.20-jre8`，如果未自定义配置仓库，则默认在下载的时候，会在镜像前面加上DockerHub 的地址。

### 常用参数

- -a, --all-tags=true|false : 是否获取仓库中所有镜像，默认为否；

- --disable-content-trust : 跳过镜像内容的校验，默认为 true;

## images命令

通过使用如下两个命令，列出本机已有的镜像：

```bash
docker images 
docker image ls
```

各个选项说明:
 - REPOSITORY：表示镜像的仓库源
 - TAG：镜像的标签
 - IMAGE ID：镜像ID
 - CREATED：镜像创建时间
 - SIZE：镜像大小

## save命令

### 保存一个镜像

```bash
docker save tomcat:9.0.20-jre8-alpine -o tomcat9.tar 
docker save tomcat:9.0.20-jre8-slim > tomcat9.slim.tar
```

### 多个镜像

```bash
docker save \ 
ubuntu:20.04 \ 
alpine:3.12.1 \ 
debian:10.6-slim \ 
centos:7.8.2003 \ 
-o linux.tar 

docker save \ 
tomcat:9.0.20-jre8-alpine \ 
tomcat:9.0.20-jre8-slim \ 
tomcat:9.0.20-jre8 \ 
-o tomcat9.0.20.tar
```

### 常用参数

- -o :输出到的文件

## load命令

```bash
docker load -i linux.tar 
docker load < tomcat9.0.20.tar
```

### 常用参数

- -input , -i : 指定导入的文件。
- -quiet , -q : 精简输出信息。

## search命令

不推荐使用search命令查找镜像，不够直观。

```bash
docker search tomcat
```

## inspect命令

- 通过 docker inspect 命令，我们可以获取镜像的详细信息，其中，包括创建者，各层的数字摘要
  等。
- docker inspect 返回的是 JSON格式的信息，如果您想获取其中指定的一项内容，可以通过 -f 来指
  定，如获取镜像大小

```bash
docker inspect tomcat:9.0.20-jre8-alpine 
docker inspect -f {{".Size"}} tomcat:9.0.20-jre8-alpine
```

## history命令

通过 docker history命令，可以列出各个层的创建信息

```b
docker history tomcat:9.0.20-jre8-alpine
```

## tag命令

标记本地镜像，将其归入某一仓库。先简单熟悉一下tag命令，后边的章节会详细进行讲解。

```bash
docker tag tomcat:9.0.20-jre8-alpine lagou/tomcat:9
```

## rmi命令

通过如下两个都可以删除镜像：

```
docker rmi tomcat:9.0.20-jre8-alpine 
docker image rm tomcat:9.0.20-jre8-alpine
```

### 常用参数

- -f, -force : 强制删除镜像，即便有容器引用该镜像；
- -no-prune : 不要删除未带标签的父镜像；

### 通过ID删除镜像

除了通过标签名称来删除镜像，我们还可以通过制定镜像 ID, 来删除镜像。一旦制定了通过 ID 来删除镜
像，它会先尝试删除所有指向该镜像的标签，然后在删除镜像本身。

```bash
docker rmi ee7cbd482336
```

#### 第一次试验

```bash
根据tomcat:9.0.20-jre8-alpine镜像重新打一个新的tag 
docker tag tomcat:9.0.20-jre8-alpine lagou/tomcat:9 

通过images命令查看镜像 
docker images 

通过image的ID删除镜像 
docker rmi 387f9d021d3a 

错误信息如下： 
Error response from daemon: conflict: unable to delete 387f9d021d3a (must be forced) - image is referenced in multiple repositories
```

#### 第二次试验

```
根据tomcat:9.0.20-jre8-alpine镜像重新打两个个新的tag 

docker tag tomcat:9.0.20-jre8-alpine lagou/tomcat:9 
docker tag tomcat:9.0.20-jre8-alpine lagou/tomcat:9.1 

根据image名称删除tomcat:9.0.20-jre8-alpine 
docker rmi tomcat:9.0.20-jre8-alpine 

通过image的ID删除镜像 
docker rmi 387f9d021d3a 

错误信息如下： 
Error response from daemon: conflict: unable to delete 387f9d021d3a (must be forced) - image is referenced in multiple repositories
```

#### 总结

- 推荐通过image的名称删除镜像
- image的ID在终端长度未完全显示，ID值会出现重复

### 删除镜像的限制

删除镜像很简单，但也不是我们何时何地都能删除的，它存在一些限制条件。当通过该镜像创建的容器
未被销毁时，镜像是无法被删除的。

##### 我们一般不推荐`-f`子命令暴力的做法，正确的做法应该是：

1. 先删除引用这个镜像的容器；
2. 再删除这个镜像；

### **清理镜像**

我们在使用 Docker 一段时间后，系统一般都会残存一些临时的、没有被使用的镜像文件，可以通过以

下命令进行清理。执行完命令后，还是告诉我们释放了多少存储空间！

```
docker image prune
```

#### 常用参数

- -a, --all : 删除所有没有用的镜像，而不仅仅是临时文件；
- -f, --force ：强制删除镜像文件，无需弹出提示确认；