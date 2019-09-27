存放一些客户端的配置文件，路径`/opt/swoole/config`，请勿删除相关文件

* config_ip.conf 配置服务端IP
* config_port.conf 配置服务端端口

>[danger] 如果遇到应用监控/应用追踪无信息，可先检查以上两个配置文件是否正确

* config_white_list.conf 配置白名单
>[success] 默认为`"App","Controller","Model","wp-includes"`文件夹名称，如果你匹配其他的文件夹，需要修改此文件。优先级从左到右，如果在`App`中获取到相关数据后就不会再给后匹配。
* config_common.conf 配置服务端客户端通讯方式和阻塞检测耗时