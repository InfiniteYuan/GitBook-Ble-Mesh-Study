---
description: >-
  A BLE mesh is a collection of up to 32,767 physical BLE devices which send and
  receive messages to triggerpredefined behaviors in the participating nodes.
---

# Overview

## Overview

A BLE mesh is a collection of up to 32,767 physical BLE devices which send and receive messages to trigger predefined behaviors in the participating nodes. 

> BLE mesh是多达32,767个物理BLE设备的集合，这些设备发送和接收消息以触发参与节点中的预定义行为。

BLE mesh is a managed flood network which broadcasts \(BLE advertisement\) a message to other nodes that are in range. The receiving nodes re-broadcast \(or relay\) the message to other nodes that are also in range. This continues until the message time to live \(TTL\) expires; once expired, nodes that receive the message no longer re-broadcast it. If a node receives a message that it previously received, it can immediately discard it by matching it against entries in the message cache. 

> BLE mesh 是一个受管理的洪水网络，它向范围内的其他节点广播（BLE 广播）消息。接收节点将消息重新广播（或中继）到范围内的其他节点。这一直持续到消息生存时间（TTL）到期为止。一旦过期，接收到该消息的节点将不再重新广播它。如果节点接收到它先前收到的消息，则可以通过将其与消息缓存中的条目进行匹配来立即将其丢弃。

BLE mesh networks use relay nodes rather than nodes that function as routers. Routers can be a single point of failure that can then result in a full network failure. By not relying on routers, these mesh networks that use flooding are far more reliable. BLE mesh messages can be received from any node within range, even if an intermediary node fails or is removed from the network. 

> BLE mesh 网络使用中继节点，而不是充当路由器的节点。路由器可以是单点故障，然后可能导致整个网络故障。通过不依赖路由器，这些使用泛洪的网状网络更加可靠。即使中间节点发生故障或已从网络中删除，BLE 消息也可以从范围内的任何节点接收。

Heartbeat messages are sent periodically to indicate a node is still alive and how many hops it may be from another node. This information can be used to optimise the TTL value. 

> 心跳消息会定期发送，以指示一个节点仍处于活动状态，以及它可能来自另一个节点的跳数。此信息可用于优化 TTL 值。

BLE mesh does not make use of connections but rather uses the BLE broadcasts \(advertisements\) introduced with BT4.0. This does not mean that all BT4.0 devices automatically support mesh \(the OS likely needs to expose a new API\) but devices that are BT4.0 devices and newer have the potential to support BLE mesh.

> BLE mesh 不使用连接，而是使用 BT4.0 引入的 BLE 广播。这并不意味着所有 BT4.0 设备都自动支持网格（操作系统可能需要公开新的 API），但是 BT4.0 设备及更高版本的设备有可能支持 BLE mesh。

## Topology

All nodes in a BLE mesh can transmit and receive messages but some nodes may have one or more specific features.

* Relay node – Can receive and rebroadcast a message 
* Low power node – Spends most of its time in a low power state with its radio turned off 
* Friend node – Works alongside a low power node by storing and forwarding messages intended for a low power node when polled by the low power node 
* Proxy node – Devices without a mesh stack can interact with devices in a BLE mesh using a GATT bearer to a proxy node.

![](.gitbook/assets/specification_node_feature.png)

## Publish/Subscribe

Messages are sent and received using a publish/subscribe paradigm. An outgoing message is published. The only exception is when an acknowledgment message is sent to a specific node that received the behavior-invoking message. Given the advert-based, managed-flood nature of message transmission, the following is a valid question: How long does it take for the message to arrive at the destination? Anecdotally, the best answer is: messages travel at the speed of sound. 

> 使用发布/订阅范式发送和接收消息。 外发消息已发布。 唯一的例外是将确认消息发送到接收到行为调用消息的特定节点时。 考虑到消息传输是基于广告的，管理洪水的性质，以下是一个有效的问题：消息到达目的地需要多长时间？ 有趣的是，最好的答案是：消息以声音的速度传播。

Each message consists of an opcode and context data. The opcode dictates the behavior at the receive end; the data can be up to 380 octets. By the time the application receives a mesh message, it is completely decrypted and given to the app as a plaintext message.

> 每个消息都包含一个操作码和上下文数据。 操作码指示接收端的行为； 数据最多可以包含380个八位位组。 在应用程序收到网格消息时，它已被完全解密并作为纯文本消息提供给应用程序。

