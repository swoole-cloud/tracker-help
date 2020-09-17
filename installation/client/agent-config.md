存放一些客户端的配置文件，路径`/opt/swoole/config`，请勿删除相关文件

* config_ip.conf 配置服务端IP
* config_port.conf 配置服务端端口

>[danger] 如果遇到应用监控/应用追踪无信息，可先检查以上两个配置文件是否正确

* config_white_list.conf 配置白名单

>[danger] 首先说明一下此配置是做什么的：
此配置是用来智能生成`调用统计`中的接口名称的。具体什么是智能生成接口名称，可参考[此视频教程](https://course.swoole-cloud.com/course-video/49)的8分30秒有讲解。
智能接口生成是依赖业务代码的文件名的，只有函数定义在这些文件名里面才会生成智能接口，默认的关键字为`"App", "Controller", "Model", "wp-includes"`。
以`"App"`为例，当你的一个请求调用了`App\Controller\Index.php`中的`test()`方法，而此test方法调用了PDO的查库语句，因为`App\Controller\Index.php`包含`"App"`关键字，所以生成的接口名为：`App\Controller\Index::test()[(PDO::query)]`。

当想调整默认的文件名关键字的时候，就可以修改此`config_white_list.conf`配置文件，并重启Fpm/Swoole进程。

优先级从左到右，区分大小写，如果在`App`中获取到相关数据后就不会再给后匹配。

* config_common.conf 配置服务端客户端通讯方式和阻塞检测耗时
```json
{"protocol":"TCP","block_time_out":"10"}
```
> 默认检测阻塞 10ms 的数据，大于这个值的系统调用就认为是阻塞的，修改后需要重启 NodeAgent
