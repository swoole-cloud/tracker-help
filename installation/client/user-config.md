我们提供了用户可自行修改客户端配置，文件路径为：`/opt/swoole/node-agent/src/node.ini`

请勿随意修改，目前可修改以下配置：

```ini
[agent]
#是否打开机器上报开关
system_report = 1
#机器系统信息上报间隔时间　单位/s
system_report_interval = 60
#worker进程数
system_worker_num = 1

#文件监控上报间隔时间 单位/s
[file_monitor_interval]
#调用统计
stats = 10
#链路追踪
trace = 10
#profile
xhprof = 10
#阻塞检查
block = 10
#内存泄漏检查
mem_leak = 10
#调用栈
bt = 10
```

>[success] 修改配置后需要重启`NodeAgent`！！！
