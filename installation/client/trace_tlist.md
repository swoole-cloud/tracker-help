## Zipkin

从 `v3.3.0` 版本开始支持链路追踪上报到`Zipkin`中，可以通过`Zipkin`的 UI 查看链路追踪的详情。

![](https://cdn.jsdelivr.net/gh/sy-records/staticfile/images/202108/zipkin-1.png)

![](https://cdn.jsdelivr.net/gh/sy-records/staticfile/images/202108/zipkin-2.png)

或者其他支持 `Zipkin` 协议的服务商，如阿里云：

![](https://cdn.jsdelivr.net/gh/sy-records/staticfile/images/202108/aliyun-zipkin-1.png)

![](https://cdn.jsdelivr.net/gh/sy-records/staticfile/images/202108/aliyun-zipkin-2.png)

使用方法如下：

修改`node.ini`文件，参考 [自定义配置](user-config.md)

修改其中的`zipkin`配置项：`host`、`port`以及`api`，修改完成后重启 NodeAgent 服务即可。