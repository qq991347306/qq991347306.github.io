---
layout:     post
title:      RocketMQ
subtitle:   RocketMQ
date:       2020-05-22
author:     LT
header-img: img/post-bg-kuaidi.jpg
catalog: true
tags:
    - JAVA
---

## 安装

- #### 准备工作
  - RocketMQ官网地址为：http://rocketmq.apache.org/
  - JAVA环境
  
- #### 下载RocketMQ

  ```
  wget https://github.com/apache/rocketmq/archive/rocketmq-all-4.2.0.tar.gz
  ```

- #### 解压文件

  ```
  tar -zxvf rocketmq-all-4.2.0.tar.gz
  ```

- #### 启动Nameserver，其中/usr/local/logs/rocketmqlogs/mqnamesrv.log为RocketMQ日志文件

```
nohup sh mqnamesrv >/usr/local/logs/rocketmqlogs/mqnamesrv.log 2>&1 &
```

- #### 启动Broker

```
nohup sh mqbroker -n localhost:9876 >/usr/local/logs/rocketmqlogs/broker.log 2>&1 &
```

