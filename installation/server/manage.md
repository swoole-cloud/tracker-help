执行命令进入容器
```bash
docker exec -it -e LINES=$(tput lines) -e COLUMNS=$(tput cols) swoole-admin-fpm-0 bash
```
进入容器后，可执行以下命令管理Server进程

## 停止/启动
```bash
ps -ef|grep trace-server|grep -v grep|awk '{print $2}'|xargs kill -9
php /opt/swoole/wwwroot/swoole-admin/center/server/trace-server/server.php

ps -ef|grep aop-server|grep -v grep|awk '{print $2}'|xargs kill -9
php /opt/swoole/wwwroot/swoole-admin/center/server/aopnet_svr/server.php

ps -ef|grep sysinfo_server|grep -v grep|awk '{print $2}'|xargs kill -9
php /opt/swoole/wwwroot/swoole-admin/center/server/sysinfo_svr/server.php

ps -ef|grep stats_server|grep -v grep|awk '{print $2}'|xargs kill -9
php /opt/swoole/wwwroot/swoole-admin/center/server/stats_svr/server.php 

ps -ef|grep autocheck_server |grep -v grep|awk '{print $2}'|xargs kill -9
php /opt/swoole/wwwroot/swoole-admin/center/server/autocheck_svr/server.php 

ps -ef|grep alert_server |grep -v grep|awk '{print $2}'|xargs kill -9
php /opt/swoole/wwwroot/swoole-admin/center/server/alert_svr/server.php
```