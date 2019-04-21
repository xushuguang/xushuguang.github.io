---
layout: post
title:  apollo 源码部署
date:   2019-04-21 16:52:00 +0800
categories: document
tag: 教程
---

* content
{:toc}

# apollo源码部署
------------------
#### 创建ApolloPortalDB
- 地址

```
https://github.com/ctripcorp/apollo/blob/master/scripts/db/migration/portaldb/V1.0.0__initialization.sql
```


#### 创建ApolloConfigDB
- 地址

```
https://github.com/ctripcorp/apollo/blob/master/scripts/db/migration/configdb/V1.0.0__initialization.sql
```


#### 配置
- 源码下载

```
https://github.com/ctripcorp/apollo
```

- 配置数据库连接信息(==修改script/build.sh==)

```
#apollo config db info
apollo_config_db_url=jdbc:mysql://{ip}:{port}/ApolloConfigDB?useSSL=false&characterEncoding=utf8
apollo_config_db_username=用户名
apollo_config_db_password=密码

#apollo portal db info
apollo_portal_db_url=jdbc:mysql://{ip}:{port}/ApolloPortalDB?useSSL=false&characterEncoding=utf8
apollo_portal_db_username=用户名
apollo_portal_db_password=密码
```

- 配置各环境meta service地址(==修改script/build.sh==)

```
${env}_meta=http://${config-service-url:port}
```

- ==注意==

```
分布式部署的时候，apollo-configservice和apollo-adminservice需要把自己的IP和端口注册到Meta Server（apollo-configservice本身）。

Apollo客户端和Portal会从Meta Server获取服务的地址（IP+端口），然后通过服务地址直接访问。

所以如果实际部署的机器有多块网卡（如docker），或者存在某些网卡的IP是Apollo客户端和Portal无法访问的（如网络安全限制），那么我们就需要在apollo-configservice和apollo-adminservice中做相关限制以避免Eureka将这些网卡的IP注册到Meta Server。

有以下两种解决办法

1.修改startup.sh，通过JVM System Property在运行时传入

-Deureka.instance.ip-address=${指定的IP}

2.修改apollo-adminservice或apollo-configservice 的bootstrap.yml文件

eureka:
  instance:
    ip-address: ${指定的IP}
    
在scripts/startup.sh中按照实际的环境设置一个JVM内存，例如：

export JAVA_OPTS="-server -Xms6144m -Xmx6144m -Xss256k -XX:MetaspaceSize=128m -XX:MaxMetaspaceSize=384m -XX:NewSize=4096m -XX:MaxNewSize=4096m -XX:SurvivorRatio=18"
```



#### 编译


```
./build.sh
```

#### 部署

- 部署apollo-configservice


```
1.获取zip压缩包
cd apollo-configservice/target/
2.上传到部署环境
3.解压
4.运行
cd apollo-configservice/script/
./startup.sh
```

- 部署apollo-adminservice


```
1.获取zip压缩包
cd apollo-adminservice/target/
2.上传到部署环境
3.解压
4.运行
cd apollo-adminservice/script/
./startup.sh
```

- 部署apollo-portal


```
1.获取zip压缩包
cd apollo-portal/target/
2.上传到部署环境
3.解压
4.运行
cd apollo-portal/script/
./startup.sh
```



