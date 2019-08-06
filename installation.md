## 服务端 - Admin后台

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
```bash
dnf install docker-ce
```

### 安装

1.  进入 `swoole-admin-docker`目录
2.  执行 `./build.sh`
3.  执行 `./run.sh`（服务端默认端口是9666，如要修改请执行 `SWOOLE_ADMIN_NGINX_PORT=端口号 ./run.sh` ）