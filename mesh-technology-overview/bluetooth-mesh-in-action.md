---
description: 'In this section, we’ll take a closer look at the Bluetooth mesh working.'
---

# Bluetooth Mesh in Action

## Message Publication and Delivery 

A network which uses Wi-Fi is based around a central network node called a router, and all network traffic passes through it. If the router is unavailable, the whole network becomes unavailable. 

In contrast, Bluetooth mesh uses a technique known as **managed flooding** to deliver messages. Messages, when published by a node, are broadcast rather than being routed directly to one or more specific nodes. All nodes receive all messages from nodes that are in direct radio range and, if configured to do so, will then relay received messages. Relaying involves broadcasting the received message again, so that other nodes, more distant from the originating node, might receive the message broadcast. 

> 使用 Wi-Fi 的网络围绕一个称为路由器的中心网络节点为基础，所有网络流量都通过该节点。如果路由器不可用，则整个网络将不可用。
>
> 相比之下，蓝牙 mesh 网络使用一种称为管理泛洪的技术来传递消息。由节点发布的消息将被广播，而不是直接路由到一个或多个特定节点。所有节点都接收来自自身无线电范围内的节点的所有消息，如果配置为这样做，则将中继接收到的消息。中继涉及再次广播接收到的消息，以便距离原始节点较远的其他节点能接收到广播的消息。

## Multipath Delivery 

An important consequence of Bluetooth technology’s use of managed flooding is that **messages arrive at their destination via multiple paths through the network**. This makes for a highly reliable network and it is the primary reason for having opted to use a flooding approach rather than routing in the design of Bluetooth mesh networking.

> 蓝牙技术使用管理泛洪的一个重要后果是，消息会通过网络中的多条路径到达目的地。这形成了高度可靠的网络，这是在蓝牙 mesh 网络设计中选择使用泛洪方法而非路由的主要原因。

## Managed Flooding 

Bluetooth mesh networking leverages the strengths of the flooding approach and optimizes its operation such that it is both reliable and efficient. The measures which optimize the way flooding works in Bluetooth mesh networking are behind the use of the term “**managed flooding**”. Those measures are as follows: 

> 蓝牙 mesh 网络利用了泛洪方法的优势，并优化了其操作，使其既可靠又高效。在蓝牙 mesh 网络中优化泛洪工作方式的措施是使用术语“管理泛洪”的背后。这些措施如下：

### Heartbeats 

Heartbeat messages are transmitted by nodes **periodically**. A heartbeat message indicates to other nodes in the network that the node sending the heartbeat is still active. In addition, heartbeat messages contain data which allows receiving nodes to **determine how far away** the sender is, in terms of **the number of hops** required to reach it. This knowledge can be exploited with the TTL field. 

> 心跳消息由节点定期发送。心跳消息向网络中的其他节点指示发送心跳的节点仍处于活动状态。此外，心跳消息还包含数据，这些数据使接收节点可以根据到达发送方所需的跳数确定发送方的距离。可以通过 TTL 字段利用此数据。

### TTL 

TTL \(Time To Live\) is a field which all Bluetooth mesh PDUs include. It controls the maximum number of hops, over which a message is relayed. **Setting the TTL allows nodes to exercise control over relaying and conserve energy, by ensuring messages are not relayed further than is required.** 

**Heartbeat messages allow nodes to determine what the optimum TTL value should be for each message published.** 

> TTL（生存时间）是所有蓝牙 Mesh PDU 都包含的字段。它控制中继的最大跳数。设置 TTL 可以确保节点不会中继太多，从而使节点可以控制中继并节省能量。
>
> 心跳消息使节点可以确定每个发布的消息的最佳 TTL 值。

### Message Cache 

A message cache must be implemented by all nodes. The cache contains all **recently** seen messages and if a message is found to be in the cache, indicating the node has seen and processed it before, it is immediately **discarded**. 

> 消息缓存必须由所有节点实现。消息缓存包含所有最近收到的消息，如果发现一条消息存在于消息缓存中，表明该节点之前已经收到和处理过该消息，则立即将其丢弃。

### Friendship 

Probably the most significant optimization mechanism in a Bluetooth mesh network is provided by the combination of **Friend nodes** and **Low Power nodes**. As described, Friend nodes provide a **message store** and **forward service** to associated Low Power nodes. This allows Low Power nodes to operate in a highly energy-efficient manner. 

> 蓝牙 mesh 网络中最重要的优化机制可能是 Friend 节点和 Low Power 节点的组合。如所述，Friend 节点提供消息存储并将服务转发到关联的 Low Power 节点。这允许低功耗节点以高效节能的方式运行。

## Traversing the Stack 

A node, receiving a message, passes it up the stack from the underlying **Bluetooth LE stack**, via the **bearer layer** to the **network layer**. 

The network layer applies various **checks** to decide whether or not to pass the message higher up the stack or to discard it.

In addition, PDUs have a Network ID field, which provides a fast way to determine which NetKey the message was encrypted with. If the NetKey is not recognized by the **network layer** on the receiving node, this indicates it does not possess the corresponding NetKey, is not a member of that subnet and so the PDU is discarded. There’s also a network **message integrity check** \(MIC\) field. If the MIC check fails, using the NetKey corresponding to the PDUs Network ID, then the message is discarded.

Messages, are received by all nodes in range of the node that sent the messages but many will be quickly discarded when it becomes apparent they are not relevant to this node due to the network or subnet\(s\) it belongs to.

The same principle is applied higher up the stack in the **upper transport layer**. Here though, the check is against the **AppKey** associated with the message, and identified by an application identifier \(AID\) field in the PDU. If the AID is unrecognized by this node, the PDU is discarded by the upper transport layer. If the transport message integrity check \(TransMIC\) fails, the message is discarded.

> 接收到消息的节点将其从底层低功耗蓝牙协议栈通过承载层传递到协议栈栈，到达网络层。
>
> 网络层应用各种检查来决定是否将消息传递到更高的协议栈栈或将其丢弃。
>
> 此外，PDU 具有网络 ID 字段，该字段提供了一种快速的方法来确定使用哪个 NetKey 对消息加密。如果接收节点上的网络层无法识别 NetKey，则表明它不具有相应的 NetKey，不是该子网的成员，因此 PDU 被丢弃。还有一个网络消息完整性检查（MIC）字段。如果 MIC 检查失败，则使用与 PDU 网络ID对应的 NetKey，则将消息丢弃。

> 消息被发送消息的节点范围内的所有节点接收，但是当这些节点所属的网络或子网而明显与它们无关时，许多消息将被迅速丢弃。
>
> 在上层传输层中，沿协议栈向上应用相同的原理。但是，此处的检查是针对与消息关联的 AppKey，并由 PDU 中的应用程序标识符（AID）字段标识。如果此节点无法识别 AID，则 PDU 被上层传输层丢弃。如果传输消息完整性检查（TransMIC）失败，则该消息将被丢弃。

