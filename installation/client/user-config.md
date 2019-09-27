我们提供了用户可自行修改客户端配置，目前可修改两种配置：

* 日志目录

>[danger] 一些记录性的日志文件会存放在此目录，如果修改请注意修改对应目录权限

* 是否上报机器信息

>[danger] 如果不需要[机器信息](../../sysinfo.md)上报的话，可以修改为`false`

* 上报进程PID列表时间

>[danger] 默认上报时间为`10s/次`

文件路径`/opt/swoole/node-agent/src/NodeAgent/Config.php`

```php
<?php

namespace NodeAgent;

class Config
{
   // 日志目录
   const NA_SWOOLE_LOGS_DIR = '/opt/swoole/logs';

   // 是否打开机器信息上报
   const NA_SYSINFO_REPORT = true;

   // sysinfo进程上报pid时间
   const NA_REPORT_PID_TIME = 10;
}
```

>[success] 修改配置后需要重启`NodeAgent`！！！