[TOC]

Swoole 企业版系统对外开放`API`接口，说明如下：

## 1\. 获取监控机器列表

接口地址：`/Http/getMachineList`
请求参数：

| 参数 | 值 | 说明 |
| --- | --- | --- |
| page | 1 | 页数，默认1 |
| pagesize | 10 | 显示数量，默认10 |

响应参数：

```json
{
    "code": 0,
    "message": "获取成功",
    "data": {
        "data": [
            {
                "id": "3", // id
                "lan_ip": "172.17.16.15", // 机器ip
                "hostname": "sy-test", // 主机名
                "cpu": {
                    "num": 1, // 核数
                    "model": "intel(r) xeon(r) cpu e5-26xx v4"
                },
                "star": "0", // 是否关注
                "extra": { // 自定义信息
                    "name": "腾讯云",
                    "out_ip": "127.0.0.3"
                },
                "time": 1557806007 // 最后上报时间
            },
            {
                "id": "2",
                "lan_ip": "172.31.202.116",
                "hostname": "iZj6cf6lmymbaq0c5nykafZ",
                "cpu": {
                    "num": 1,
                    "model": "intel(r) xeon(r) cpu e5-2682 v4 @ 2.50ghz"
                },
                "star": "0",
                "extra": {},
                "time": 0 // 机器创建了但是没有上报机器信息
            }
        ],
        "count": 2, // 机器总数
        "success": 2, // 运行中的机器
        "error": 0, // 停止运行的机器,
        "error_node": [ // 停止运行的机器
            "192.168.2.181"
        ],
        "totalpage": 1 // 总页数
    }
}
```

## 2\. 标识关注机器

接口地址：`/Http/setMachineStar`
请求参数：

| 参数 | 值 | 说明 |
| --- | --- | --- |
| ip | 172.17.0.3 | 机器IP，必填 |
| star | 1或0 | 1 关注 0 不关注，必填 |

响应参数：

```json
{
    "code": 0,
    "message": "添加关注成功",
    "data": true
}

{
    "code": 0,
    "message": "取消关注成功",
    "data": true
}
```

## 3\. 获取关注服务器列表及相应CPU使用率

接口地址：`/Http/getMachineCpu`
请求参数：

| 参数 | 值 | 说明 |
| --- | --- | --- |
| page | 1 | 页数，默认1 |
| pagesize | 6 | 显示数量，默认6 |

响应参数：

```json
{
    "code": 0,
    "message": "获取成功",
    "data": {
        "data": [
            {
                "_node": "172.17.16.15", // 机器ip
                "_time": "2019-03-29 11:39:01",
                "_value0": 3.42, // cpu使用率
                "cpu": 1 // cpu核数
            }
        ],
        "totalpage": 1 // 总页数
    }
}
```

## 4\. 获取主机当前情况

TCP连接数，TCP最大连接数；CPU核数，CPU使用率；网络出入流量；进程数；磁盘读写IO次数、util；磁盘使用情况；内存大小，内存使用情况（总使用，缓存内容，空闲内存）

接口地址：`/Http/getSysinfo`

请求参数：

| 参数 | 值 | 说明 |
| --- | --- | --- |
| ip | 172.17.16.15 | 机器IP，必填 |

响应参数：

```json
{
    "code": 0,
    "message": "获取成功",
    "data": {
        "cpu": 1, // CPU核数
        "use_cpu": 3.47, // CPU使用率
        "tcp_num": "6", // TCP连接数
        "tcp_max": "14",  // TCP最大连接数
         "io": [ // 磁盘IO
              {
                  "name": "io_sda",
                  "io_r": 0, // 读
                  "io_w": 3.84, // 写
                  "io_util": 0.27 // until
              }
          ],
        "disk": [ // 磁盘使用情况
            {
                "name": "disk_sda1",
                "use": 31.96, // 使用量 百分比
                "free": 68.04, // 空闲量 百分比
                "all": 49.09 // 磁盘容量 G
            }
        ],
        "mem": { // 内存
            "total": 991.76,
            "free": 95.27, 
            "available": 536.68,
            "buffers": 68.28,
            "cached": 481.18
        },
        "net": [ // 网络出入流量
            {
                "name":"net_eth0",
                "out": "1672", // 出网
                "in": "1420" // 入网
            }
        ],
        "process": [ // 进程
            {
                "pid": "19234", // pid
                "status": "0", // 状态 1 忙 0 空闲
                "type": "FPM", // fpm和cli
                "value": 3.5 // 内存占用
            },
            {
                "pid": "30822",
                "status": "0",
                "type": "FPM",
                "value": 6.38
            },
            {
                "pid": "30824",
                "status": "0",
                "type": "FPM",
                "value": 11.31
            },
            {
                "pid": "30825",
                "status": "0",
                "type": "FPM",
                "value": 11.43
            },
            {
                "pid": "16703",
                "status": "0",
                "type": "CLI",
                "value": 11.43
            }
        ],
        "count_process": 5 // 进程总数
    }
}
```

## 5\. 获取应用

接口地址：`/Http/getModuleList`

请求参数：

| 参数 | 值 | 说明 |
| --- | --- | --- |
| project | 9 | 项目ID（接口8获取），必填 |

响应参数：

```json
{
    "code": 0,
    "message": "获取成功",
    "data": [
        {
            "id": "8", // ID
            "name": "www.php-app.com", // 名称
            "type": "1", 
            "alias_name": "" // 别名
        },
        {
            "id": "34",
            "name": "test.qq52o.cn",
            "type": "1",
            "alias_name": ""
        }
    ]
}
```

## 6\. 获取应用下的接口信息

接口地址：`/Http/getInterfaceByModuleId`

请求参数：

| 参数 | 值 | 说明 |
| --- | --- | --- |
| module\_id | 8 | 应用ID，上一个接口获取的，必填 |
| page | 1 | 页数，默认1 |
| pagesize | 10 | 显示数量，默认10 |

响应参数：

```json
{
    "code": 0,
    "message": "获取成功",
    "data": {
        "data": [
            {
                "id": "119700", // id
                "name": "/blog/detail/54", // 接口名
                "alias": "/blog/detail/54||1", // 别名
                "addtime": "2019-05-01 12:57:03" // 添加时间
            }
        ],
        "totalpage": 15618 // 总页数
    }
}
```

## 7\. 获取接口详情

接口地址：`/Http/getInterfaceInfo`

请求参数：

| 参数 | 值 | 说明 |
| --- | --- | --- |
| module\_id | 8 | 应用ID，必填 |
| interface\_id | 463 | 接口ID，必填 |
| date\_key | 2019-03-29 | 日期，必填 |
| hour\_start | 02 | 开始时间 |
| hour\_end | 10 | 结束时间 |

响应参数：

```json
{
    "code": 0,
    "message": "获取成功",
    "data": [
        {
            "id": "975",
            "interface_id": "463",
            "module_id": "34",
            "instance_id": "21",
            "req_type": "2",
            "time_key": "138",
            "date_key": "2019-03-29", // 日期
            "total_count": "4", // 调用次数
            "fail_count": "0", // 失败次数
            "total_time": "723",
            "total_fail_time": "0",
            "avg_time": "181", // 平均响应时间
            "avg_fail_time": "0", // 失败平均时间
            "max_time": "288", // 响应最大值ms
            "min_time": "97", // 响应最小值
            "fail_server": "",
            "succ_server": "{\"121.46.248.88\":2,\"221.230.143.58\":1,\"221.230.143.101\":1}",
            "total_server": "{\"121.46.248.88\":2,\"221.230.143.58\":1,\"221.230.143.101\":1}",
            "fail_client": "",
            "succ_client": "{\"121.227.207.179\":4}",
            "total_client": "{\"121.227.207.179\":4}",
            "ret_code": "",
            "cost_time": "{\"5\":0,\"10\":0,\"50\":0,\"100\":1,\"200\":1,\"500\":2,\"1000\":0,\"1000+\":0}",
            "succ_ret_code": "{\"200\":4}",
            "succ_count": 4, // 成功次数
            "succ_rate": 100, // 成功率
            "time_str": "11:30 ~ 11:35", // 时间
            "interface_name": "Index.php:app\\\\index\\\\controller\\\\Index-&gt;myCurl-&gt;curl_exec"
        }
    ]
}
```

## 8\. 获取项目id

接口地址：`/Http/getProjectId`

请求参数：

| 参数 | 值 | 说明 |
| --- | --- | --- |
| page | 1 | 页数，默认1 |

响应参数：

```json
{
    "code": 0,
    "message": "获取成功",
    "data": {
        "data": [
            {
                "id": "19", // 项目id
                "name": "默认项目",
                "ckey": "default_web_project",
                "owner_uid": "0",
                "intro": "所有自动创建的模块都会默认在此项目中，可以移动到其他项目中",
                "add_time": "2019-04-22 19:17:14"
            },
            {
                "id": "9", 
                "name": "基础服务",
                "ckey": "platform",
                "owner_uid": "94",
                "intro": "基础支撑业务",
                "add_time": "2018-10-07 22:32:56"
            }
        ],
        "totalpage": 1 //总页数
    }
}
```

## 9\. 机器扩展信息

可针对某台机器增加扩展信息，也可按扩展信息进行分类

### 9.1 获取数据

接口地址：`/Http/optionsExtra`

* 获取某一台机器的扩展信息

请求参数：

| 参数 | 值 | 说明 |
| --- | --- | --- |
| ip | 172.17.16.15 | 机器IP |

响应参数：

```json
{
    "code": 0,
    "message": "获取成功",
    "data": {
        "name": "阿里云", // 自定义字段，内容
        "out_ip": "127.0.0.1"
    }
}
```

* 获取所有机器扩展信息

请求参数：

| 参数 | 值 | 说明 |
| --- | --- | --- |
| type | name | 自定义字段(必填，可为空)，根据此字段分类 |
| value | 阿里云 | 自定义字段对应的内容，根据此字段统计数量 |

以实例给予说明如下：

>[info] 请求： `/Http/optionsExtra?type`

响应参数：

```json
{
    "code": 0,
    "message": "获取成功",
    "data": {
        "data": [ // 机器信息
            {
                "lan_ip": "172.17.0.4", // 机器ip
                "extra": { // 自定义内容
                    "name": "腾讯云",
                    "out_ip": "127.0.0.2"
                }
            },
            {
                "lan_ip": "192.168.2.171",
                "extra": {
                    "name": "腾讯云",
                    "out_ip": "127.0.0.3",
                    "ceshi": "1"
                }
            },
            {
                "lan_ip": "192.168.2.181",
                "extra": []
            },
            {
                "lan_ip": "172.17.16.15",
                "extra": {
                    "name": "阿里云",
                    "out_ip": "127.0.0.1"
                }
            },
            {
                "lan_ip": "172.31.202.116",
                "extra": []
            },
            {
                "lan_ip": "172.31.0.4",
                "extra": []
            }
        ],
        "node": [], // 以下3个参数在现在这种情况下始终为空。
        "keys": [],
        "count": 0
    }
}
```

>[info] 请求： `/Http/optionsExtra?type=name`

响应参数：

```json
{
    "code": 0,
    "message": "获取成功",
    "data": {
        "data": [ // 机器信息
            {
                "lan_ip": "172.17.0.4", // 机器ip
                "extra": { // 自定义内容
                    "name": "腾讯云",
                    "out_ip": "127.0.0.2"
                }
            },
            {
                "lan_ip": "192.168.2.171",
                "extra": {
                    "name": "腾讯云",
                    "out_ip": "127.0.0.3",
                    "ceshi": "1"
                }
            },
            {
                "lan_ip": "192.168.2.181",
                "extra": []
            },
            {
                "lan_ip": "172.17.16.15",
                "extra": {
                    "name": "阿里云",
                    "out_ip": "127.0.0.1"
                }
            },
            {
                "lan_ip": "172.31.202.116",
                "extra": []
            },
            {
                "lan_ip": "172.31.0.4",
                "extra": []
            }
        ],
        "node": [], // 一直为空
        "keys": [ // 根据name字段分类
            "腾讯云",
            "阿里云"
        ],
        "count": 0 // 一直为0
    }
}
```

>[info] 请求： `/Http/optionsExtra?type=name&value=阿里云`

响应参数：

```json
{
    "code": 0,
    "message": "获取成功",
    "data": {
        "data": [ // 机器信息
            {
                "lan_ip": "172.17.0.4", // 机器ip
                "extra": { // 自定义内容
                    "name": "腾讯云",
                    "out_ip": "127.0.0.2"
                }
            },
            {
                "lan_ip": "192.168.2.171",
                "extra": {
                    "name": "腾讯云",
                    "out_ip": "127.0.0.3",
                    "ceshi": "1"
                }
            },
            {
                "lan_ip": "192.168.2.181",
                "extra": []
            },
            {
                "lan_ip": "172.17.16.15",
                "extra": {
                    "name": "阿里云",
                    "out_ip": "127.0.0.1"
                }
            },
            {
                "lan_ip": "172.31.202.116",
                "extra": []
            },
            {
                "lan_ip": "172.31.0.4",
                "extra": []
            }
        ],
        "node": [ // 当name等于阿里云时的机器ip
            "172.17.16.15"
        ],
        "keys": [ // name分类
            "腾讯云",
            "阿里云"
        ],
        "count": 1 // 当name等于阿里云时的机器总数
    }
}
```

### 9.2 添加数据

该接口需要 `POST` 请求，⚠️每次请求会进行覆盖写

>[warning] 请自行根据 `9.1 获取数据` 中的 `获取某一台机器的扩展信息` 处理数据

接口地址： `/Http/optionsExtra`

请求参数：

| 参数 | 值 | 说明 |
| --- | --- | --- |
| ip | 172.17.16.15 | 机器IP |
| options | name=腾讯云,out\_ip=127.0.0.3 | 要设置的自定义信息，以英文逗号分割 |

响应参数：

```json
{
    "code": 0,
    "message": "设置成功",
    "data": ""
}

{
    "code": 1,
    "message": "设置失败",
    "data": ""
}
```