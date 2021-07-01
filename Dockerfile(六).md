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

-a :提交的镜像作者；
-c :使用Dockerfile指令来创建镜像；
-m :提交时的说明文字；
-p :在commit时，将容器暂停。
小例子
结合docker cp命令自定义nginx的index页面
官网地址
Dockerfile其实就是我们用来构建Docker镜像的源码，当然这不是所谓的编程源码，而是一些命令的集
合，只要理解它的逻辑和语法格式，就可以很容易的编写Dockerfile。简单点说，Dockerfile可以让用户
个性化定制Docker镜像。因为工作环境中的需求各式各样，网络上的镜像很难满足实际的需求。