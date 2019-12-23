## 日志数据
提供了删除服务端日志的脚本，默认保留7天日志，在Docker外以下命令执行即可。
```bash
docker exec swoole-admin-fpm-0 bash -c "/opt/swoole/wwwroot/swoole-admin/bin/clean-log.sh"
```
## 数据库数据