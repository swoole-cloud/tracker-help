>[success] 高级版或旗舰版支持此功能，详情请联系 [客服](contact-us.md) 。

默认存储数据为`MySQL`，应用追踪数据量过大，所以支持使用`clickhouse`进行存储。

## 安装clickhouse

具体安装步骤，可参照`clickhouse`官方文档：[部署运行](https://clickhouse.yandex/docs/zh/getting_started/)

## 配置信息

### 用户名

安装时会要求配置用户名，默认的用户名为`default`

### 密码

安装完成后默认无密码

>[danger] 因`clickhouse`安装配置不一定相同，所以这里不做任何限制，部署前需要修改相关配置信息。文件路径`swoole-tracker/swoole-admin-docker/docker/swoole-admin-common-conf/clickhouse.php`

## 添加数据表
请将`swoole-tracker/swoole-admin-docker/swoole-admin/sql/clickhouse.sql`文件中的`SQL`添加进`clickhouse`当中
