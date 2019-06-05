---
layout: post
title:  kafka demo
date:   2019-06-05 16:52:00 +0800
categories: document
tag: 教程
---

* content
{:toc}

### 前言

用于测试demo使用，不考虑集群等各种性能影响，适用于初学者个人测试开发
    
### 安装

为了方便快捷进行测试，使用 docker 进行安装部署
    
- vim  docker-compose.yml


```
version: '2'
services:
  zookeeper:
    image: wurstmeister/zookeeper
    volumes:
      - ./data:/data
    ports:
      - "2181:2181"

  kafka:
    image: wurstmeister/kafka
    ports:
      - "9092:9092"
    environment:
      KAFKA_ADVERTISED_HOST_NAME: 106.15.185.148
      KAFKA_MESSAGE_MAX_BYTES: 2000000
      KAFKA_CREATE_TOPICS: "Topic1:1:3,Topic2:1:1:compact"
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
    volumes:
      - ./kafka-logs:/kafka
      - /var/run/docker.sock:/var/run/docker.sock

  kafka-manager:
    image: sheepkiller/kafka-manager
    ports:
      - 9020:9000
    environment:
      ZK_HOSTS: zookeeper:2181
```
- 启动

```
docker-compose up -d
```

-  注意
1. kafka-manager是用于管理kafka的一个工具
2. 确保服务器的端口2181和9092没有被占用并且可以访问（如果被占用请自行修改）


### springboot 集成

- pom文件添加依赖

```
        <dependency>
            <groupId>org.springframework.kafka</groupId>
            <artifactId>spring-kafka</artifactId>
        </dependency>
```

-  applicaiton.yml配置生产者消费者

```
spring:
  kafka:
    producer:
      bootstrap-servers: {kafka ip}:{port}
    consumer:
      enable-auto-commit: true
      group-id: applog
      auto-offset-reset: latest
      bootstrap-servers: {kafka ip}:{port}
```

-  程序主入口添加注解


```
    @EnableKafka
```

-  编写生产者service

```
@Service
public class KafkaProducer {

    @Autowired
    private KafkaTemplate kafkaTemplate;

    public void sendTest(String message) {
        kafkaTemplate.send("test", message+",kafka  " +
                LocalDateTime.now().format(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss.SSS")));
    }
}
```
-  编写消费者service

```
@Service
public class KafkaConsumer {

    private static Logger logger = LoggerFactory.getLogger(KafkaConsumer.class);

    @KafkaListener(topics = {"test"})
    public void consumer0(String message){
        logger.error("consumer0 topic message : {}", message);
    }
    @KafkaListener(topics = {"test"})
    public void consumer1(String message){
        logger.error("consumer1 topic message : {}", message);
    }
}
```

-  编写Controller,用于启动生产者生产消息

```
@RestController
public class DemoController {

    private static Logger logger = LoggerFactory.getLogger( DemoController.class );

    
    @GetMapping("/kafka")
    public String kafka() {

        logger.error("KafkaScheduled...start");
        for (int i=0 ; i<100000; i++){
            kafkaSender.sendTest("kafka"+i+": test");
        }


        return "kafka";
    }
}
```
-  启动程序，访问，查看打印日志


```
http://localhost:8761/kafka
```


```
 .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v2.1.4.RELEASE)

2019/05/19-18:05:44 [http-nio-8761-exec-1] ERROR com.james.apollodemo.controller.DemoController- KafkaScheduled...start
2019/05/19-18:05:44 [org.springframework.kafka.KafkaListenerEndpointContainer#1-0-C-1] ERROR com.james.apollodemo.service.KafkaConsumer- consumer1 topic message : kafka0: test,kafka  2019-05-19 18:05:44.254
2019/05/19-18:05:44 [org.springframework.kafka.KafkaListenerEndpointContainer#1-0-C-1] ERROR com.james.apollodemo.service.KafkaConsumer- consumer1 topic message : kafka1: test,kafka  2019-05-19 18:05:44.326
2019/05/19-18:05:44 [org.springframework.kafka.KafkaListenerEndpointContainer#1-0-C-1] ERROR com.james.apollodemo.service.KafkaConsumer- consumer1 topic message : kafka2: test,kafka  2019-05-19 18:05:44.327
2019/05/19-18:05:44 [org.springframework.kafka.KafkaListenerEndpointContainer#1-0-C-1] ERROR com.james.apollodemo.service.KafkaConsumer- consumer1 topic message : kafka3: test,kafka  2019-05-19 18:05:44.327
2019/05/19-18:05:44 [org.springframework.kafka.KafkaListenerEndpointContainer#1-0-C-1] ERROR com.james.apollodemo.service.KafkaConsumer- consumer1 topic message : kafka4: test,kafka  2019-05-19 18:05:44.327
2019/05/19-18:05:44 [org.springframework.kafka.KafkaListenerEndpointContainer#1-0-C-1] ERROR com.james.apollodemo.service.KafkaConsumer- consumer1 topic message : kafka5: test,kafka  2019-05-19 18:05:44.327
2019/05/19-18:05:44 [org.springframework.kafka.KafkaListenerEndpointContainer#1-0-C-1] ERROR com.james.apollodemo.service.KafkaConsumer- consumer1 topic message : kafka6: test,kafka  2019-05-19 18:05:44.327
2019/05/19-18:05:44 [org.springframework.kafka.KafkaListenerEndpointContainer#1-0-C-1] ERROR com.james.apollodemo.service.KafkaConsumer- consumer1 topic message : kafka7: test,kafka  2019-05-19 18:05:44.327
2019/05/19-18:05:44 [org.springframework.kafka.KafkaListenerEndpointContainer#1-0-C-1] ERROR com.james.apollodemo.service.KafkaConsumer- consumer1 topic message : kafka8: test,kafka  2019-05-19 18:05:44.327
2019/05/19-18:05:44 [org.springframework.kafka.KafkaListenerEndpointContainer#1-0-C-1] ERROR com.james.apollodemo.service.KafkaConsumer- consumer1 topic message : kafka9: test,kafka  2019-05-19 18:05:44.327

```