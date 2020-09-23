[TOC]

>[success] 上报成功后在后台查看，需要切换当前项目，默认在默认项目中。

![](../images/screenshot_1578041268650.png)

>[danger] 可以先查看链路追踪，首页和调用统计需要时间进行分析。

# FPM应用

<!--### 自动创建应用-->

默认tracker客户端装了扩展和agent，并发生了请求，就会自动上报，服务端自动生成对应的应用，并上报监控数据。如果在后台关闭了自动创建应用（默认是开启自动创建的），则需要手动创建应用。

<!--### 手动创建应用

在服务端->系统管理->相应项目->应用管理->新增应用 应用名即为您要监控站点的域名，如有端口请加上端口。

>[info] 例如：您想监控的站点域名为`www.test.com`，服务名则填`www.test.com`（注意：域名若带端口，服务名也要带端口）

配置完成后，稍等片刻即可查看对应的监控数据-->


# Service应用(Cli模式)


### 自动创建应用

如果您的Cli应用是 `Swoole` 的 `HttpServer` 那么会在发生请求后自动生成应用，名称为`host:port`

如果您的Cli应用是 `Swoole` 的其他类型`Server` 那么会在发生请求后自动生成应用，名称为`ip:port`
 >[danger] 自动创建应用不支持Swoole的多端口监听，也不支持[协程风格的Swoole服务端](https://wiki.swoole.com/#/server/co_init)。
<!--###
### 手动创建应用

在服务端->系统管理->相应项目->应用管理->新增应用 应用名即为您要监控的服务名。

>[info] 例如：您想监控服务名为`user_service`的cli常驻进程应用，您的应用类型选择Service，服务名填`user_service`.
-->

### 手动创建应用
cli下有两种需求需要手动创建应用：
- 自动创建的应用的**名称**不满足需求
- 不满足自动创建应用的条件，需要手动才能创建应用（例如使用的是Workerman）

手动创建需要调用2个API：
```php
 /**
 * 被调用开始前执行
 * @param  $func eg.  'App\Login\Weibo::login'
 * @param  $serviceName 应用名称 例如'userService'
 * @param  $serverIp eg. '192.1.1.1'
 * @return StatsCenter_Tick object
 */
$tick = \SwooleTracker\Stats::beforeExecRpc($func, $serviceName, $serverIp);

/*
 * 被调用结束后执行
 * @param $tick  StatsCenter_Tick object
 * @param $ret   true/false
 * @param $errno 201
 * @return void
 */
\SwooleTracker\Stats::afterExecRpc($tick, $ret, $errno);
```
微服务框架肯定都有统一的服务入口，在服务入口处(开始)加上`beforeExecRpc()`方法，服务出口处(结束)加上`afterExecRpc()`方法。此时后台可以统计到服务的所有链路信息，例如这次调用的 `mysql` `redis` 等调用都会在`afterExecRpc()`后上报，**这两个API除了用来生成应用，还可以用来透传**，下文介绍。





# 透传TraceId/SpanId
透传的作用是为了分布式trace，举个例子，浏览器请求机器A的FPM，然后机器A的FPM调用了机器B的Swoole服务，正常情况下这会生成2个trace信息，如果你想让A=>B的所有信息合并成一个trace，一览无余的查看整个链路追踪，就需要把A的TraceId和SpanId透传给服务B，透传的情况又分为四种：
- `FPM/Cli` 调用通过`Curl`调用另外一个`FPM`服务
这种情况什么也不用做，tracker扩展会自动将TraceId/SpanId加到http的header里面实现透传。
- `FPM/Cli` 通过`Curl`调用另外一个`Cli`的服务
这种情况需要在Cli的服务端的服务入口和出口加上两个函数，分别是beforeExecRpc和afterExecRpc：
    ```php
    //伪代码
    $traceId = $header['x-swoole-traceid'];//装了tracker扩展的curl请求会自动带上x-swoole-traceid这个header
    $spanId = $header['x-swoole-spanid'];//同上
    //执行开始函数，把traceId和spanId传给这个函数
    $tick = \SwooleTracker\Stats::beforeExecRpc($func, $serviceName, $serverIp, $traceId, $spanId);
    /*
     *  执行真正的业务逻辑
     */
    \SwooleTracker\Stats::afterExecRpc($tick, $ret, $errno);//调用结束函数
    ```
-  `FPM/Cli` 通过 Swoole的`Http/Client` 调用另外一个`Cli`的服务：
在rpc**被调用**端我们的做法同上，不再赘述，在rpc调用端我们需要做一些工作：  
    ```php
    //在Rpc调用的入口加上如下代码，将traceId和spanId塞到header中
    $client->setHeaders(array_merge(
        [
            'x-swoole-traceid' => getSwooleTrackerTraceId(),
            'x-swoole-spanid' => genSwooleTrackerSpanId(),
        ],
        $client->requestHeaders
    ));
    /*
    * 进行rpc请求
    */
    ```
-  `FPM/Cli` 通过 `原生TCP协议` 调用另外一个`Cli`的服务：
这个例子有很多，比如`thrift，grpc，tars`, 还有`自定义的rpc协议`等等，此种情况我们需要在调用端和服务端的入口都进行埋点。
在Rpc调用端：
 
    ```php
    //伪代码
    $traceId =  getSwooleTrackerTraceId();//获取当前traceId
    $spanId = genSwooleTrackerSpanId();//生成一个新的spaId
    //开始请求前调用，注意函数名和Rpc服务端的不一样
    $tick = \SwooleTracker\Stats::beforeReqRpc($func, $serviceName, $serverIp);
    
    /*
    * 进行rpc请求
    */
    $testClient = new TestClient();
    //把$traceId和$spanId自己想办法带到服务端
    $r = $testClient->client->hello('swoole!'. '|'. $traceId . '|'. $spanid);
    
    
    //rpc请求结束后调用，注意函数名和Rpc服务端的不一样
    \SwooleTracker\Stats::afterReqRpc($tick, $ret, $errno);
   ```  

    在Rpc服务端：

    ```php
    //伪代码
    $recv = explode("|", $data);
    $traceId = $recv[1];
    $spanId = $recv[2];//从数据中解析出这两个id，也可以自己改rpc协议，直接加到协议头
    //执行开始函数，把traceId和spanId传给这个函数
    $tick = \SwooleTracker\Stats::beforeExecRpc($func, $serviceName, $serverIp, $traceId, $spanId);
    /*
     *  执行真正的业务逻辑
     */
    \SwooleTracker\Stats::afterExecRpc($tick, $ret, $errno);//调用结束函数
       ```
