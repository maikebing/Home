---
title: iotsharp
date: 2020-11-04 23:39:55
---

![IOTSharp LOGO](https://static.oschina.net/uploads/img/202010/23114443_umAb.png)

IoTSharp是一个用于数据收集通过规则引擎流转数据最后通过数据可视化管理数据结果，以及租户结构的资产管理。可用于实现自主可控的、自有机房、无需支付高额租赁费用的自有物联网平台， 支持分布式， 这意味着你可以通过多台服务器共同处理数据，遥测数据进行分开存储，单表方式、 分表、时序数据库等。 主要功能如下：

1. 支持基于MQTT、CoAP、HTTP协议的数据采集协议
2. 支持X509加密验证和用户名密码 以及批量token认证
3. 提供STM32 基于 rt-thread 的采集sdk
4. 提供树莓派中基于C#的采集sdk 
5. 提供常规其他linux中采集sdk
6. 支持数字孪生概念， 因此数据区分为属性和遥测数据， 遥测数据存储在时序数据中
7. 通过EFCore.Sharding支持了分表存储， 默认是按月存储，根据数据量， 你可以修改为按日， 按时 。
8. 通过Maikebing.Data.Taos 我们支持了涛思数据的时序数据库 TDengine ， Maikebing.Data.Taos 是目前.Net 生态中唯一最完整的TDengine 支持组件。
9. 内置了 ZeroMQ 服务， 用于支持基于ZeroMQ的分布式消息处理。
10. 通过CAP实现了EventBus 消息总线， 通过CAP.Extensions 支持了ZeroMQ的消息总线支持， 可以做到纯粹.Net 生态。
11. 通过CAP实现了 消息数据 能在 MongoDB LiteDB PostgreSql中存储。
12. 通过CAP实现了消息可以通过RabbitMQ Kafka ZeroMQ 进行生产和消费。 当多台服务器时， 一台可以作为主服务器， 其他可以作为辅助服务器用以处理所有采集数据。

在此新版本发布之时 ， 我们在生产环境已经使用，本月数据已经达到三千多万，两千台设备， 五万多个检测项。

## 如何安装 ?

```
 mkdir iotsharp
cd iotsharp 
wget https://raw.githubusercontent.com/IoTSharp/IoTSharp/master/docker-compose.yml
docker-compose up -d  
```

演示版本:

[http://139.9.232.10:2927](http://139.9.232.10:2927/)

## 我们的docker镜像

- docker pull iotsharp/iotsharp

## 如何安装为Linux服务 ?

- mkdir /var/lib/iotsharp/
- cp ./* /var/lib/iotsharp/
- chmod 777 /var/lib/iotsharp/IoTSharp
- cp iotsharp.service /etc/systemd/system/iotsharp.service
- sudo systemctl enable /etc/systemd/system/iotsharp.service
- sudo systemctl start iotsharp.service
- sudo journalctl -fu iotsharp.service

# 关联项目

## IoTSharp.SDKs

IoTSharp.SDKs 包含了 IoTSharp.Sdk.MQTT IoTSharp.Sdk.Http 用于采集端或者边缘部分进行数据采集通过sdk发送给IoTSharp。 https://github.com/IoTSharp/IoTSharp.SDKs

## IoTSharp-C-Client-Sdk

IoTSharp-C-client-Sdk 是一个mqtt客户端， 针对iotsharp封装了协议 

https://github.com/IoTSharp/IoTSharp-C-Client-Sdk

## paho.mqtt.c's demo

 也是一个mqtt的协议实现，演示了如何使用 paho.mqtt.c 对接IoTSharp https://github.com/IoTSharp/IoTSharp.Edge.paho.mqtt.c

## IoTSharp.Edge.nanoFramework

IoTSharp.Edge.nanoFramework 是一个nanoFramework 下实现的mqtt客户端， 意味着你可以用C#在STM32上对接IoTSharp 

https://github.com/IoTSharp/IoTSharp.Edge.nanoFramework

 更多信息请阅读 

https://www.cnblogs.com/MysticBoy/p/13159648.html 

https://www.nanoframework.net/

## IoTSharp.Edge.RT-Thread

IoTSharp.Edge.RT-Thread (STM32L4 + Wi-Fi, sensor, lcd, audio etc) 是一个在RT-Thread 上进行对接的演示， 如果您项目中采集设备使用了RTT， 这个演示将是个好的开头。 

https://github.com/IoTSharp/IoTSharp.Edge.RT-Thread

|                                                              |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ![20190615010003.jpg](https://static.oschina.net/uploads/img/202010/23114443_4VXb.jpg) | ![20190615010115.jpg](https://static.oschina.net/uploads/img/202010/23114443_1lW5.jpg) |

IoTSharp依赖生态

IoTSharp的依赖生态是为了实现IoTSharp而改进或者创建的项目。 这些项目是组成IoTSharp的关键部分。 与此同时， 针对其他项目也有一定需求。

- MaiKeBing.CAP.ZeroMQ [![MaiKeBing.CAP.ZeroMQ](https://static.oschina.net/uploads/img/202010/23114445_eZ0y.svg)](https://www.nuget.org/packages/MaiKeBing.CAP.ZeroMQ/)

  ZeroMQ（也称为 ØMQ，0MQ 或 zmq）是一个可嵌入的网络通讯库（对 Socket 进行了封装）。 它提供了携带跨越多种传输协议（如：进程内，进程间，TCP 和多播）的原子消息的 sockets 。 有了ZeroMQ，我们可以通过发布-订阅、任务分发、和请求-回复等模式来建立 N-N 的 socket 连接。 ZeroMQ 的异步 I / O 模型为我们提供可扩展的基于异步消息处理任务的多核应用程序。当前组件使用了NetMQ 为CAP提供了 发布-订阅， 推送-拉取两种消息模式。 示例请参见Sample.ZeroMQ.InMemory， 当测试 推送-拉取 消息模式时 ， 可以启动 Sample.ConsoleApp 可以测试负载均衡。

- MaiKeBing.CAP.LiteDB [![MaiKeBing.CAP.LiteDB](https://static.oschina.net/uploads/img/202010/23114446_NawC.svg)](https://www.nuget.org/packages/MaiKeBing.CAP.LiteDB/)

  [LiteDB](http://www.litedb.org/)是一个小型的.NET平台开源的NoSQL类型的轻量级文件数据库。特点是小和快，dll文件只有200K大小，而且支持LINQ和命令行操作，数据库是一个单一文件，类似Sqlite。为CAP存储了本地文件的NoSQL存储方式， 示例请参见 Sample.LiteDB.InMemory

- MaiKeBing.HostedService.ZeroMQ [![MaiKeBing.HostedService.ZeroMQ](https://static.oschina.net/uploads/img/202010/23114447_mfPO.svg)](https://www.nuget.org/packages/MaiKeBing.HostedService.ZeroMQ/)

  将ZeroMQ作为HostedService 运行， 通过配置可以实现Pub-Sub、Push-Pull 两种分发模式

- IoTSharp.X509Extensions [![IoTSharp.X509Extensions](https://static.oschina.net/uploads/img/202010/23114447_ZfxO.svg)](https://www.nuget.org/packages/IoTSharp.X509Extensions/)

  是一个自签名证书组件， 用于支持IoTSharp的X509加密部分

  https://github.com/IoTSharp/IoTSharp.X509Extensions

- MQTTnet.AspNetCoreEx [![MQTTnet.AspNetCoreEx](https://static.oschina.net/uploads/img/202010/23114448_VNi5.svg)](https://www.nuget.org/packages/MQTTnet.AspNetCoreEx/)

 是一个用于支持mqttnet 认证的独立组件! https://github.com/maikebing/MQTTnet.AspNetCoreEx

- Silkier [![Silkier](https://static.oschina.net/uploads/img/202010/23114449_NjTA.svg)](https://www.nuget.org/packages/Silkier/)

  Silkier 是一个常用函数集合的组件

- Silkier.EFCore [![Silkier.EFCore](https://static.oschina.net/uploads/img/202010/23114449_26Pm.svg)](https://www.nuget.org/packages/Silkier.EFCore/)

  Silkier.EFCore用来支持一些sql语句的执行， 以及一些转换等等。 
  https://github.com/maikebing/Silkier

- SilkierQuartz [![SilkierQuartz](https://static.oschina.net/uploads/img/202010/23114450_Hc5s.svg)](https://www.nuget.org/packages/SilkierQuartz/)

  SilkierQuartz 是一个改造了的Quartz的中间件 

https://github.com/maikebing/SilkierQuartz

- Maikebing.EntityFrameworkCore.Taos [![Maikebing.EntityFrameworkCore.Taos](https://static.oschina.net/uploads/img/202010/23114451_2KoQ.svg)](https://www.nuget.org/packages/Maikebing.EntityFrameworkCore.Taos/)

  TDengine的支持组件。 

  https://github.com/maikebing/Maikebing.EntityFrameworkCore.Taos