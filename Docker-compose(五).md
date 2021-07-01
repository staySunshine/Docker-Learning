# docker-compose

## docker compose是什么

Docker Compose能够在 Docker 节点上，以单引擎模式（Single-Engine Mode）进行多容器应用的部署和管理。多数的现代应用通过多个更小的微服务互相协同来组成一个完整可用的应用。部署和管理繁多的服务是困难的。而这正是 Docker Compose 要解决的问题。Docker Compose是通过一个声明式的配置文件描述整个应用，从而使用一条命令完成部署。

## yml配置文件

Docker Compose 使用 YAML 文件来定义多服务的应用。YAML 是 JSON 的一个子集，因此也可以使用JSON。
Docker Compose 默认使用文件名 docker-compose.yml。当然，也可以使用 -f 参数指定具体文件。
Docker Compose 的 YAML 文件包含 4 个一级 key：version、services、networks、volumes

- version 是必须指定的，而且总是位于文件的第一行。它定义了 Compose 文件格式（主要是API）的版本。注意，version 并非定义 Docker Compose 或 Docker 引擎的版本号。
- services 用于定义不同的应用服务。上边的例子定义了两个服务：一个名为 xie-mysql数据库服务以及一个名为xie-eureka的微服。Docker Compose 会将每个服务部署在各自的容器中。
- networks 用于指引 Docker 创建新的网络。默认情况下，Docker Compose 会创建 bridge 网络。
  这是一种单主机网络，只能够实现同一主机上容器的连接。当然，也可以使用 driver 属性来指定不
  同的网络类型。
- volumes 用于指引 Docker 来创建新的卷。

### yml配置实列

``` yaml
version: '3'
services:
  xie-mysql:
    build:
      context: ./mysql
    environment:
      MYSQL_ROOT_PASSWORD: admin
    restart: always
    container_name: xie-mysql
    volumes:
    - /data/edu-bom/mysql/lagou:/var/lib/mysql
    image: mysql:5.7
    ports:
      - 3306:3306
    networks:
      xie-net:
  xie-eureka:
    build:
    context: ./edu-eureka-boot
    restart: always
    ports:
      - 8761:8761
    container_name: edu-eureka-boot
    hostname: edu-eureka-boot
    image: edu-eureka-boot:1.0
    depends_on:
      - xie-mysql
    networks:
      xie-net:
networks:
  xie-net:
volumes:
  xie-vol:
```

## 常用命令汇总

### 启动服务

```
docker-compose up
docker-compose up -d
```

### 停止服务

```
docker-compose down
```

### 列出所有运行容器

```
docker-compose ps
```

### 查看服务日志

```
docker-compose logs
```

### 构建或者重新构建服务

```
docker-compose build
```

### 启动服务

```
docker-compose start
```

### 停止已运行的服务

```
docker-compose stop
```

### 重启服务

```
docker-compose restart
```

### 官网地址

```
https://docs.docker.com/compose/reference/build/
```

