`Swoole Tracker`在 3.0+版本推出了完全免费的内存检测工具，方便广大 PHPer 定位自己 Cli 程序的内存问题：

## 内存泄漏检测：

不同于[调试器](debuger.md)中的内存泄漏检测，3.0+的泄漏检测理论更先进，具体用法参考[此篇文章](https://mp.weixin.qq.com/s?__biz=MzIyMDkxMTIwNA==&mid=2247483839&idx=1&sn=ead68d219925b7a17602cc37219be056&chksm=97c582b4a0b20ba21a43721172ef6500442a4abfe6a95b878f82df9dc26518dbb4d1f3182c9e&mpshare=1&scene=23&srcid=09152cYGQgLTmZEuUQwaJAQt&sharer_sharetime=1600152472209&sharer_shareid=fcf66e3aa3ef36f1935f2c3ab766f0a0#rd)。

## 内存申请 TopN 检测：

有的 PHP 代码没有内存泄漏，但是代码中如果有特别大的内存申请，这会带来两个问题：一是大内存申请会有性能问题，二是在一瞬间可能达到 PHP 的 `memory_limit` 限制而报错，`Swoole Tracker v3.0.1+`版本增加了这种行为的分析功能，用法和上文的`内存泄漏检测`类似，下面是例子：

```php
$http->on("request", function($request, $response) {
    trackerHookMalloc();//标记为循环体函数，之后每次请求都会打日志

    $req->test = str_repeat("big string", 1024);//超大内存申请
    $response->end("hello");
});
```

上述代码生成日志后，在任意位置执行`php -r "trackerAnalyzeMalloc(5);"` 会输出每次请求的最大的几次内存申请，其中的参数`5`代表只输出前 5 的大内存申请。