# 应用管理
[TOC]
## 新增应用

* 系统管理->项目管理->应用管理->新增应用
![应用管理.png](images/1552255630467-acfefbd6-d61b-43e0-a0af-dbfa7546373a.png)
![新增应用.png](images/1552255817765-597535e0-c89d-404f-8449-d126c8fceb9f.png)
![新增应用2.png](images/watermark,type_d3F5LW1pY3JvaGVp,size_10,text_6K-G5rKD572R57uc54mI5p2D5omA5pyJ,color_FFFFFF,shadow_50,t_80,g_se,x_10,y_10-20190806131146152.png)

- 应用类型支持`Web`和`Service`

> Service应用为常驻进程类的应用。

- 请求状态条件和应用状态条件为首页(应用面板)服务状态的监控阈值，可根据实际需求配置。
- 服务端和客户端部署完成后，默认会自动创建web应用到默认项目中。

## 修改应用

* 系统管理->项目管理->应用管理->修改按钮
![image.png](images/1552257318345-f55aa23b-e7d4-4d68-a72b-dc3303fa094d-20190806131635137.png)

### 别名

可自定义别名，别名在左侧菜单栏中优先显示。

![image.png](images/watermark,type_d3F5LW1pY3JvaGVp,size_10,text_6K-G5rKD572R57uc54mI5p2D5omA5pyJ,color_FFFFFF,shadow_50,t_80,g_se,x_10,y_10-20190806131653084.png)

## 删除应用

* 系统管理->项目管理->应用管理->删除按钮

![删除应用.png](images/1552257917681-26dd9031-27ff-4c10-bd9b-aa75f64a6c3f.png)

## 合并应用

合并相同功能的应用，比如`www`和不带`www`的内容相同，并未`301`重定向时可进行合并，展示在一起。

系统管理->项目管理->应用管理->合并应用

![image.png](images/watermark,type_d3F5LW1pY3JvaGVp,size_14,text_6K-G5rKD572R57uc54mI5p2D5omA5pyJ,color_FFFFFF,shadow_50,t_80,g_se,x_10,y_10-20190806131721452.png)

### 新增合并

系统管理->项目管理->应用管理->合并应用->增加合并

![image.png](images/watermark,type_d3F5LW1pY3JvaGVp,size_14,text_6K-G5rKD572R57uc54mI5p2D5omA5pyJ,color_FFFFFF,shadow_50,t_80,g_se,x_10,y_10-20190806131730387.png)

## 应用黑名单

添加进应用黑名单的 `Web` 应用不会自动创建，如果需要请先自动创建应用后，选择加入黑名单后，删除该应用即可。

系统管理->项目管理->应用管理->合并黑名单->新增黑名单

![image.png](images/watermark,type_d3F5LW1pY3JvaGVp,size_14,text_6K-G5rKD572R57uc54mI5p2D5omA5pyJ,color_FFFFFF,shadow_50,t_80,g_se,x_10,y_10-20190806131741192.png)

## 移动应用

新增移动多个应用，可以同时移动多个应用到其他项目中

系统管理->项目管理->应用管理->移动应用

![image.png](images/watermark,type_d3F5LW1pY3JvaGVp,size_14,text_6K-G5rKD572R57uc54mI5p2D5omA5pyJ,color_FFFFFF,shadow_50,t_80,g_se,x_10,y_10-20190806131800316.png)