[TOC]
## 依赖
### 安装docker和docker-compose
系统依赖docker和docker-compose.

如果不了解如何部署docker，使用以下命令来进行安装（需要root权限）

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
```

优先使用你的发行版提供的docker-compose进行安装
例如
```bash
dnf install docker-compose # fedora
yum install docker-compose # CentOS 7/ RHEL7
apt-get install docker-compose # debian及其变种如Ubuntu
apk add docker-compose # alpine
pacman -S docker-compose # arch
```
如果你的发行版没有提供docker-compose（例如CentOS 6），
docker-compose二进制可以从https://github.com/docker/compose/releases下载
> 注意：docker-compose可能依赖python3
### 启用docker daemon
对于使用systemd的发行版（fedora，CentOS/RHEL7，debian及其变种，arch）：
```bash
systemctl start docker # 开启dockerd
systemctl enable docker # 启用dockerd的开机启动
```
> 注意：有的发行版的docker daemon的systemd单元名称不是docker，需要自行决定start和enable的名称

对于使用openrc的发行版（alpine，gentoo）
```bash
rc-service docker start # 开启dockerd
rc-update add docker # 启用dockerd的开机启动
```
对于其他sysvinit-like的启动系统
```bash
/etc/init.d/docker start
```
参考你的发行版提供的启动管理器机制来启用开机自启
### CentOS 8
centos8的官方仓库提供的是podman作为docker的替代，不完全兼容docker和docker-compose，可能存在问题，考虑使用一般的docker来进行安装:
```bash
dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo
dnf install docker-ce
```

## 安装

1. 进入 `swoole-admin-docker`目录
2. 执行 `./build.sh`
3. 执行 `./run.sh`（服务端默认端口是9666，如要修改请执行 `SWOOLE_ADMIN_NGINX_PORT=端口号 ./run.sh` ）

## 访问Admin后台

运行`./run.sh`后 直接访问当前机器的`ip:9666`(或自定义的端口)即可，默认用户名是`admin`密码为`admin`

>[danger] 安装完成后首次访问如果报错`500`，请将`data`目录删掉，执行`./kill.sh ./rm.sh`后，重新执行安装步骤。

## 关闭Admin后台

在 `swoole-admin-docker` 目录执行 `./stop.sh`

### 脚本说明

```shell
build.sh # 构建脚本
run.sh # 启动admin
stop.sh # 停止admin
restart.sh # 重启admin进程
kill.sh # 强制关闭admin进程
rm.sh # 删除admin的容器
```

## 重装

如果不想保留之前的数据，`rm`掉整个 `swoole-admin-docker` 目录，如果想保留之前的数据请保留 `swoole-admin-docker/data` 目录

>[info] 升级版本可能会涉及数据库文件，请删掉`swoole-admin-docker/data`目录

然后再解压压缩包，执行`./build.sh ./run.sh`

>[danger] 重装后需要重启客户端的`node-agent`进程、`fpm`进程以及`Service`进程；

## 细节

1. `./build.sh`会自动创建好镜像
2. `./run.sh`会启动四个容器，分别是`php-fpm(swoole-admin)`, `mysql`, `redis`, `nginx` , 数据库为已经初始化状态的, 可以开箱即用
