[TOC]

## 直接部署

### 1. 安装对应的`swoole_tracker`扩展

在 `php.ini` 中加入以下配置
```ini
extension=swoole_tracker.so 
apm.enable=1           #打开总开关
apm.sampling_rate=100  #采样率 100%

#开启内存泄漏检测需要添加
apm.enable_memcheck=1  #开启内存泄漏检测 默认0 关闭
```
>[danger] `enable`为 1 时表示应用监控百分百拦截并上报
> `sampling_rate`采样率只作用于应用追踪，设置为 100 则表示每次请求都会生成一条 trace 数据

### 2. 卸载不兼容扩展

1. xdebug
2. ioncube loader
3. zend guard loader
4. xhprof

### 3. 运行

在命令行中执行 `./deploy_env.sh 127.0.0.1`。(`127.0.0.1`为admin后台的机器ip)

### 4. 重启服务

重启swoole server或者php-fpm服务，发生请求后稍等片刻，等待服务端接收客户端发送的数据。

## 在Docker部署

>[danger] 请注意修改相关路径为你自己的路径！！！以下的swoole.so只是演示说明可安装其他扩展，swoole_tracker不依赖swoole扩展

在docker环境部署需要修改Dockerfile或者docker-compose.yml或者在`docker run`命令中添加参数，以下以采用官方docker-compose v3.7配置文件格式，php:fpm-7.x(-alpine)镜像为例，描述如何在docker部署

### 修改Dockerfile以部署node-agent

在Dockerfile中执行deploy_env.sh来部署node-agent，然后在entrypoint中添加node-agent，例如

```dockerfile
# dockerfile的其他部分

# 部署node-agent
ADD swoole-tracker-vx.y.z.tar.gz /tmp/
RUN tar -C / -xvf /tmp/swoole-tracker-vx.y.z.tar.gz && \
    cd /swoole-tracker/node-agent && \
    ./deploy_env.sh 服务端IP && \
    rm /tmp/swoole-tracker-vx.y.z.tar.gz

# 添加entrypoint脚本
RUN printf '#!/bin/sh\n/opt/swoole/script/php/swoole_php /opt/swoole/node-agent/src/node.php &\nphp-fpm $@' > /opt/swoole/entrypoint.sh && \
    chmod 755 /opt/swoole/entrypoint.sh

# 启用entrypoint脚本（-x方便调试， 可以去掉）
ENTRYPOINT [ "sh", "-x", "/opt/swoole/entrypoint.sh" ]
```

### 启用扩展

对于官方镜像php:fpm系列，php(-fpm)默认读取/usr/local/etc/php/conf.d下的配置文件，默认的entrypoint会将"-"开头的参数作为fpm启动参数，因此可以采用以下方式启用swoole_tracker扩展

在Dockerfile添加配置文件：

```dockerfile
RUN printf 'extension=/path/to/swoole.so\nextension=/path/to/swoole_tracker7x.so\n' > /usr/local/etc/php/conf.d/swoole-tracker.ini
```

或在docker-compose.yml添加启动参数

```dockerfile
services:
  your-service:
    build:
      context: cgi-docker
      dockerfile: Dockerfile
    image: myphpfpm:1
    command:
      - "-dextension=/path/to/swoole.so"
      - "-dextension=/path/to/swoole_tracker7x.so"
```

或在docker run命令中添加启动参数

```dockerfile
docker run --other-arguments myphpfpm:1 -dextension=/path/to/swoole.so -dextension=/path/to/swoole_tracker7x.so``
```

### 配置docker安全选项

扩展中使用了默认权限不允许的系统调用，使用了docker默认seccomp配置不允许的系统调用，需要额外配置：

>[success] 参考[https://docs.docker.com/engine/security/seccomp/](https://docs.docker.com/engine/security/seccomp/)

对于权限配置，可以添加SYS_PTRACE cap，或者使用提升权限模式（不推荐）

对于seccomp，可以修改seccomp配置，或关闭seccomp配置（不推荐，这将导致docker内程序可以执行create_module，kexec_load等危险系统调用）

### 修改seccomp配置

修改seccomp配置文件（修改自[默认文件](https://github.com/moby/moby/blob/master/profiles/seccomp/default.json))）:

```bash
--- a.json
+++ b.json
# 在.syscalls[0].names中加入"ptrace"，这将允许ptrace
@@ -359,7 +359,8 @@
                                "waitid",
                                "waitpid",
                                "write",
-                               "writev"
+                               "writev",
+                               "ptrace"
                        ],
                        "action": "SCMP_ACT_ALLOW",
                        "args": [],
# 如果你的docker较新，则它已经配置了ptrace在4.8以上内核可用
# 参考https://github.com/moby/moby/commit/1124543ca8071074a537a15db251af46a5189907
# 移除这段
@@ -369,18 +370,6 @@
                },
-                {
-                        "names": [
-                               "ptrace"
-                       ],
-                       "action": "SCMP_ACT_ALLOW",
-                       "args": null,
-                       "comment": "",
-                       "includes": {
-                               "minKernel": "4.8"
-                       },
-                       "excludes": {}
-               },
               {
                       "names": [
                                "personality"
                        ],
                        "action": "SCMP_ACT_ALLOW",
```

在docker run使用该seccomp并给予SYS\_PTRACE权限：

```bash
docker run --other-arguments --cap-add=SYS_PTRACE --security-opt seccomp=/path/to/that/modified/profile.json ...
```

或docker-compose.yml中:

```yml
# 在docker-compose.yml中：
services:
  your-service:
    build:
      context: cgi-docker
      dockerfile: Dockerfile
    image: myphpfpm:1
    # 给予SYS_PTRACE权限
    cap_add:
      - "SYS_PTRACE"
    # 配置使用修改的seccomp
    security_opt:
      - "seccomp=/path/to/that/modified/profile.json"
```

### 关闭seccomp（不推荐）

与修改配置类似，但不需要创建json，将 `seccomp=/path/to/that/modified/profile.json` 换成`seccomp=unconfined`即可

## 单独的NodeAgent容器（高级用法）

此处提供一种单独运行的方法，仅供参考：

```bash
# 在host安装NodeAgent（或者手动安装/opt/swoole的文件）
cd /some/place/swoole-tracker/node-agent
./deploy_env.sh a.b.c.d
# 开启NodeAgent容器
docker run \
 --name nodeagent \
 -d --cap-add SYS_PTRACE \
 --security-opt seccomp=unconfined \
 --entrypoint /opt/swoole/script/php/swoole_php \
 -v /tmp:/tmp:rw \
 -v /opt/swoole:/opt/swoole:rw \
 alpine:edge \
 /opt/swoole/node-agent/src/node.php
# 开启cgi容器
docker run \
 --name cgi1 \
 -d \
 --pid="container:nodeagent" \
 --net="container:nodeagent" \
 -v /tmp:/tmp:rw \
 -v /opt/swoole:/opt/swoole:rw \
 -v php:7.3-fpm
```
## 管理客户端进程

查看 [常见问题](qa.md) 中的「管理NodeAgent守护进程」