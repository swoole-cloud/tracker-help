## 日志数据
提供了删除服务端日志的脚本，在Docker外执行以下命令即可。
```bash
docker exec swoole-admin-fpm-0 bash -c "/opt/swoole/wwwroot/swoole-admin/bin/clean-log.sh"
```
此脚本会保留7天以日期命名的日志文件，同时会清空swoole.log日志。

## 数据库数据
默认30天清理一次，如需清理请在Docker外执行以下命令即可，此脚本会保留最近3天的数据库数据。

```bash
docker exec swoole-admin-fpm-0 bash -c "/opt/swoole/wwwroot/swoole-admin/bin/clean-db"
```

>[info] 从`v3.2.x`版本起，增加了自定义天数功能，可以在以上两个命令中增加需要保存数据的天数，即：

```bash
docker exec swoole-admin-fpm-0 bash -c "/opt/swoole/wwwroot/swoole-admin/bin/clean-log.sh 3"

docker exec swoole-admin-fpm-0 bash -c "/opt/swoole/wwwroot/swoole-admin/bin/clean-db 5"
```