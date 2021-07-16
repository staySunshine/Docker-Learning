# Dockerfile

## Docker 创建镜像方式

1. 基于已有的镜像创建；
2. 基于 Dockerfile 来创建；
3. 基于本地模板来导入；

## 基于已有的镜像创建

commit命令
docker commit :从容器创建一个新的镜像。

### 语法

```
docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
```

### 常用参数

- -a :提交的镜像作者；
- -c :使用Dockerfile指令来创建镜像；
- -m :提交时的说明文字；
- -p :在commit时，将容器暂停。

## Dockerfile的基本结构

Dockerfile是一个包含用于组合映像的命令的文本文档。可以使用在命令行中调用任何命令。 Docker通过读取Dockerfile中的指令自动生成映像。

docker build命令用于从Dockerfile构建映像。可以在docker build命令中使用 -f 标志指向文件系统中任何位置的Dockerfile。Dockerfile由一行行命令语句组成，并且支持以#开头的注释行。

Dockerfile分为四部分：基础镜像信息、维护者信息、 镜像操作指令和容器启动时执行指令。

## Dockerfile文件说明

Docker以从上到下的顺序运行Dockerfile的指令。为了指定基本映像，第一条指令必须是FROM。一个声明以 ＃ 字符开头则被视为注释。可以在Docker文件中使用 RUN ， CMD ， FROM ， EXPOSE ， ENV 等指令。

## Dockerfile常见命令

| 命令       | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| FROM       | 指定基础镜像，必须为第一个命令                               |
| MAINTAINER | 维护者(作者)信息                                             |
| ENV        | 设置环境变量                                                 |
| RUN        | 构建镜像时执行的命令                                         |
| CMD        | 构建容器后调用，也就是在容器启动时才进行调用。               |
| ENTRYPOINT | 指定运行容器启动过程执行命令，覆盖CMD参数<br/>ENTRYPOINT与CMD非常类似，不同的是通过docker run执行的命令不会覆盖<br/>ENTRYPOINT，而docker run命令中指定的任何参数，都会被当做参数再次传递<br/>给ENTRYPOINT。Dockerfile中只允许有一个ENTRYPOINT命令，多指定时会覆<br/>盖前面的设置，而只执行最后的ENTRYPOINT指令。 |
| ADD        | 将本地文件添加到容器中，tar类型文件会自动解压(网络压缩资源不会被解压)，<br/>可以访问网络资源，类似wget |
| COPY       | 功能类似ADD，但是是不会自动解压文件，也不能访问网络资源      |
| WORKDIR    | 工作目录，类似于cd命令                                       |
| ARG        | 用于指定传递给构建运行时的变量                               |
| VOLUMN     | 用于指定持久化目录                                           |
| EXPOSE     | 指定于外界交互的端口                                         |
| USER       | 指定运行容器时的用户名或 UID，后续的 RUN 也会使用指定用户。使用USER指<br/>定用户时，可以使用用户名、UID或GID，或是两者的组合。当服务不需要管理<br/>员权限时，可以通过该命令指定运行用户。并且可以在之前创建所需要的用户 |

### build命令

**docker build** 命令用于使用 Dockerfile 创建镜像。

#### 语法

```
docker build [OPTIONS] PATH | URL | -
```

#### 常用参数

- -build-arg=[] :设置镜像创建时的变量；
- -f :指定要使用的Dockerfile路径；
- -rm :设置镜像成功后删除中间容器；
- -tag, -t: 镜像的名字及标签，通常 name:tag 或者 name 格式；可以在一次构建中为一个镜像设置多个标签

### 制作镜像-dockerfile

```dockerfile
FROM openjdk:8-alpine3.9
# 作者信息
MAINTAINER laosiji Docker springboot  laosiji@lagou.com"
# 修改源
RUN echo "http://mirrors.aliyun.com/alpine/latest-stable/main/" >
/etc/apk/repositories && \
 echo "http://mirrors.aliyun.com/alpine/latest-stable/community/" >>
/etc/apk/repositories
# 安装需要的软件，解决时区问题
RUN apk --update add curl bash tzdata && \
 rm -rf /var/cache/apk/*
#修改镜像为东八区时间
ENV TZ Asia/Shanghai
ARG JAR_FILE
COPY ${JAR_FILE} app.jar
EXPOSE 8082
ENTRYPOINT ["java","-jar","/app.jar"]
```

#### 生成测试镜像

```
docker build --rm -t lagou/dockerdemo:v1 --build-arg JAR_FILE=dockerdemo.jar .
```

#### 测试、删除镜像

```
docker run -itd --name dockerdemo -p 8080:8080 lagou/dockerdemo:v1
docker logs -f dockerdemo
http://192.168.198.100:8082
docker stop dockerdemo
docker rm dockerdemo
```

