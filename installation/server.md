# 服务端 - Admin后台

### 依赖

系统依赖 `docker` ，启用docker-ce源后，使用如下命令安装

* dpkg系Debian/Ubuntu

```bash
apt-get install docker-ce
```

* rpm系CentOS/RHEL/Fedora

```bash
yum install docker-ce
```

* rpm系，Fedora/CentOS8+/RHEL8+

```
dnf install docker-ce
```

### 安装

1. 进入 `swoole-admin-docker`目录
2. 执行 `./build.sh`
3. 执行 `./run.sh`（服务端默认端口是9666，如要修改请执行 `SWOOLE_ADMIN_NGINX_PORT=端口号 ./run.sh` ）

### 关闭Admin后台

在`swoole-admin-docker`目录执行 `./rm.sh`

### 重装

如果不想保留之前的数据，`rm`掉整个 `swoole-admin-docker`目录，如果想保留之前的数据请保留`swoole-admin-docker/data`目录

>[info] 升级版本可能会涉及数据库文件，请删掉swoole-admin-docker/data目录

然后再解压压缩包，执行`./build.sh ./run.sh`

>[danger] 重装后需要重启客户端的`node-agent`进程、`fpm`进程以及`Service`进程；删除`/tmp/swoole`下所有文件

### 细节

1. `./build.sh`会自动创建好镜像
2. `./run.sh`会启动四个容器，分别是`php-fpm(swoole-admin)`, `mysql`, `redis`, `nginx` , 数据库为已经初始化状态的, 可以开箱即用

### 访问后台

运行`./run.sh`后 直接访问当前机器的ip:9666(或自定义的)端口即可，默认用户名是`admin`密码`admin`