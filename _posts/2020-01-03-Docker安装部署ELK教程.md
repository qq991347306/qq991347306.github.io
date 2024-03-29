---


layout: post
title: Docker安装部署ELK教程
date: 2020-05-25
author: LT
category: JAVA
---

## Docker安装部署ELK教程

[elastic官网](https://www.elastic.co/cn/) 

```
# elastic官方镜像 https://www.elastic.co/cn/downloads/
docker pull docker.elastic.co/elasticsearch/elasticsearch:7.10.1
docker pull docker.elastic.co/kibana/kibana:7.10.1
docker pull docker.elastic.co/logstash/logstash:7.10.1
docker pull docker.elastic.co/beats/filebeat:7.10.1
```

1.Docker 安装 Elasticsearch

```
# 下载镜像 查看镜像
docker pull elasticsearch:7.1.1
docker images

# 创建自定义的网络(用于连接到连接到同一网络的其他服务(例如Kibana))
docker network create somenetwork

# 查看网络
docker network ls

# 运行 elasticsearch
docker run -d --name elasticsearch --net somenetwork -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:7.1.1

# 查看容器状态
docker ps

# 检测 elasticsearch 是否启动成功
curl 127.0.0.1:9200
```

2.Docker 安装 Kibana

```
# 下载镜像 查看镜像
docker pull kibana:7.1.1
docker images

# 运行 Kibana
docker run -d --name kibana --net somenetwork -p 5601:5601 kibana:7.1.1

# 查看容器启动状态
docker ps
```

汉化Kibana

```
# 进入容器
docker exec -it kibana /bin/sh

# 修改配置文件
vi /opt/kibana/config/kibana.yml
最后一行加
i18n.locale: zh-CN

# 重启镜像
docker restart kibana
```

3.Docker 安装 Logstash

```
# 下载镜像 查看镜像
docker pull logstash:7.1.1
docker images

# 运行 Logstash
docker run -it -d -p 5044:5044 --name logstash --net somenetwork logstash:7.1.1

# 查看容器启动状态
docker ps
```

配置logstash

```
docker exec -it logstash /bin/bash
vi /usr/share/logstash/pipeline/logstash.conf
```

logstash.conf

```
input {
  tcp {
    port => 5044
    codec => json_lines
  }
}

output {
  stdout {
    codec => rubydebug
  }
  elasticsearch {
    hosts=>["http://elasticsearch:9200"]
    index => "springboot-elk"
  }
}
```

重启logstash

```
docker restart logstash
```

4.Docker 安装 Filebeat

```
# 下载镜像 查看镜像
docker pull store/elastic/filebeat:7.1.1
docker images

# 运行 Filebeat
docker run -it -d --name filebeat --net somenetwork store/elastic/filebeat:7.1.1

# 查看容器启动状态
docker ps
```

