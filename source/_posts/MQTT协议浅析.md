---
title: MQTT协议浅析
date: 2023-10-26 00:21:04
tags:
- mqtt
- 主题和负载
- 发布和订阅
categories:
- 通信协议
---

## 模式

mqtt是发布/订阅的模式，客户端可以订阅感兴趣的主题，并接收与该主题相关的消息。

<!--more-->

## 结构

是中心分布，其他物联网设备连接到同一个服务端。每个物联网节点都可以作为一个发布者或者订阅者。发布者publish给服务器，订阅者subcribe从服务器。

## 消息结构

- topic，是一个字符串。当订阅者和发布者使用同名的topic时，它们之间就会自动构建连接。topic通过`/`来区分层级,如：
  `chat/room/1
  sensor/10/temperature
  sensor/+/temperature`

  MQTT  topic支持以下两种通配符：`+` 和 `#`。

  - `+`：表示单层通配符，例如 `a/+` 匹配 `a/x` 或 `a/y`。

  - `#`：表示多层通配符，例如 `a/#` 匹配 `a/x`、`a/b/c/d`。

  > **注意**：通配符主题只能用于订阅，不能用于发布。

- payload，实际传输的内容，消息的主体部分。

## 轻量化特性

MQTT 开销低、报文小的特点使其非常适合物联网设备，因为它消耗更少的资源。

## 支持许多编程语言

MQTT是一个开放的协议，因此获得了广泛的语言支持。

## broker

MQTT Broker 是负责处理客户端请求的关键组件，包括建立连接、断开连接、订阅和取消订阅等操作，同时还负责消息的转发。

## QoS(服务质量)

mqtt提供了三种服务质量，在不同网络环境下保证消息的可靠性。

- QoS 0：消息最多传送一次。如果当前客户端不可用，它将丢失这条消息。
- QoS 1：消息至少传送一次。
- QoS 2：消息只传送一次。

## 基本工作流程

1. 客户端使用TCP/IP协议与broke建立连接，可选TLS/SSL加密。
2. 客户端可以向特定topic发布消息，也可订阅topic接收消息。
3. broker接受发布的消息，并转发给订阅了对应topic的客户端。根据QoS等级确保消息可靠传递。

## 搭建mqtt broker

首先选择一个合适的MQTT服务器软件，这里推荐常用的EMQX。它适用于物联网、工业物联网和车联网。可以通过docker命令来安装emqx。

```docker
docker run -d --name emqx -p 1883:1883 -p 8083:8083 -p 8084:8084 -p 8883:8883 -p 18083:18083 emqx/emqx
```

## mqtt客户端测试

推荐使用MQTTX，它可以方便地与Broker进行交互，提升开发和调试的效率。它可以快速创建链接保存并同时建立多个连接客户端。

以下是一个[免费公共 MQTT Broker](https://www.emqx.com/zh/mqtt/public-mqtt5-broker)，它基于完全托管的 [MQTT 云服务 - EMQX Cloud](https://www.emqx.com/zh/cloud) 创建。服务器信息如下：

> Server: `broker.emqx.io`
>
> TCP Port: `1883`
>
> WebSocket Port: `8083`
>
> SSL/TLS Port: `8883`
>
> Secure WebSocket Port: `8084`

## 保留消息Retain

当 MQTT 客户端向服务器发布消息时，可以设置保留消息标志。保留消息存储在消息服务器上，后续订阅该主题的客户端仍然可以收到该消息。

## clean session

如果客户端连接时设置 Clean Session 为 false，并且使用相同的客户端 ID 再次上线，那么消息服务器将为客户端缓存一定数量的离线消息，并在它重新上线时发送给它。

## 遗嘱消息Last Will

如果 MQTT 客户端异常离线（在断开连接前没有向服务器发送 DISCONNECT 消息），MQTT 服务器会发布遗嘱消息。
