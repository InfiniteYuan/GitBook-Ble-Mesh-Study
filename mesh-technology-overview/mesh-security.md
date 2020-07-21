---
description: 'In this section, we’ll take a closer look at the Bluetooth mesh security.'
---

# Mesh Security

## Mesh Security is Mandatory

Bluetooth LE allows the profile designer to exploit a range of different security mechanisms, from the various approaches to pairing that are possible, to individual security requirements associated with individual characteristics. Security is in fact, totally optional, and its permissible to have a device which is completely open, with no security protections or constraints in place. The device designer or manufacturer is responsible for analyzing threats and determining the security requirements and solutions for their product. 

In contrast, in Bluetooth mesh, **security is mandatory**. **The network, individual applications and devices are all secure and this cannot be switched off or reduced in any way.**

> 低功耗蓝牙允许配置文件设计者利用各种不同的安全机制，从各种方法到可能的配对，再到与各个特征相关的单个安全要求。实际上，安全性是完全可选的，并且允许具有完全开放的设备，而没有任何安全保护或约束。设备设计者或制造商负责分析威胁并确定其产品的安全要求和解决方案。
>
> 相反，在蓝牙 mesh 中，安全性是强制性的。网络，单个应用程序和设备都是安全的，不能以任何方式关闭或减少。

## Mesh Security Fundamentals 

The following fundamental security statements apply to all Bluetooth mesh networks: 

1. **All** mesh messages are encrypted and authenticated. 
2. Network security, application security and device security are addressed **independently**.
3. Security keys can be **changed** during the life of the mesh network via a **Key Refresh procedure**.
4. **Message obfuscation** makes it **difficult to track messages** sent within the network providing a privacy mechanism to make it **difficult to track nodes**.
5. Mesh security protects the network **against replay attacks**.
6. The process by which devices are added to the mesh network to become nodes, **（Provisioning）is itself a secure process**.
7. Nodes can be removed from the network securely, in a way which **prevents trashcan attacks**.

> 以下基本安全声明适用于所有蓝牙 mesh 网络： 
>
> 1. 所有 mesh 消息均已加密和认证。 
> 2. 网络安全性，应用程序安全性和设备安全性是独立解决的。
> 3. 可以在 mesh 网络的生存期内通过“密钥刷新”过程来更改安全密钥。
> 4. 消息混淆使跟踪网络内发送的消息变得困难，从而提供了一种隐私机制，使跟踪节点变得困难。
> 5. mesh 安全保护网络免受重放攻击。
> 6. 将设备添加到 mesh 网络以成为节点的过程（配网）本身就是安全的过程。
> 7. 可以通过防止垃圾桶攻击的方式将节点安全地从网络中删除。

## Separation of Concerns and Mesh Security Keys

At the heart of Bluetooth mesh security are **three types of security keys**. Between them, these keys provide security to different aspects of the mesh and achieve a critical capability in mesh security, that of “**separation of concerns**”. 

To understand this and appreciate its significance, consider a mesh light which can act as a relay. In its capacity as a relay, it may find itself handling messages relating to the building’s Bluetooth mesh door and window security system. A light has no business being able to access and process the details of such messages but does need to relay them to other nodes.

To deal with this potential conflict of interest, the **mesh uses different security keys for securing messages at the network layer from those used to secure data relating to specific applications** such as lighting, physical security, heating and so on. 

**All nodes in a mesh network possess the network key \(NetKey\).** Indeed, it is possession of this shared key which makes a node a member of the network. **A network encryption key** and **a Privacy Key** are derived directly from the NetKey.

**Being in possession of the NetKey** allows a node to decrypt and authenticate **up to the Network Layer** so that network functions such as **relaying**, can be carried out. It does **not allow application data to be decrypted**.

**The network may be sub-divided into subnets and each subnet has its own NetKey**, which is possessed only by those nodes which are members of that subnet. **This might be used, for example, to isolate specific, physical areas, such as each room in a hotel.**

**Application data for a specific application can only be decrypted by nodes which possess the right application key \(AppKey\).** Across the nodes in a mesh network, there may be many distinct AppKeys but typically, each AppKey will only be possessed by a small subset of the nodes, namely those of a type which can participate in a given application. For example, lights and light switches would possess the lighting application’s AppKey but not the AppKey for the heating system, which would only be possessed by thermostats, valves on radiators and so on. 

**AppKeys are used by the upper transport layer to decrypt and authenticate messages before passing them up to the access layer.** 

**AppKeys are associated with only one NetKey.** This association is termed “**key binding**” and means that **specific applications, as defined by possession of a given AppKey, can only work on one specific network, whereas a network can host multiple, independently secure applications.**

**The final key type is the device key \(DevKey\)**. This is a special type of application key. **Each node has a unique DevKey known to the Provisioner device and no other**. The DevKey is used in **the provisioning process to secure communication between the Provisioner and the node.** 

> 蓝牙 mesh 安全性的核心是三种类型的安全性密钥。在它们之间，这些密钥为 mesh 的不同方面提供安全性，并实现了 mesh 安全性的关键功能，即“关注点分离”。
>
> 要了解这一点并理解其重要性，请考虑可以充当中继器的 mesh 灯。以中继的身份，它可能会发现自己正在处理与建筑物的蓝牙 mesh 门窗安全系统有关的消息。灯没有业务能够访问和处理此类消息的详细信息，但确实需要将它们的消息中继到其他节点。
>
> 为了处理这种潜在的利益冲突，mesh 使用不同的安全密钥来保护网络层上的消息，这些不同的密钥用于保护与特定应用（例如照明，物理安全，加热等）有关的消息。
>
> mesh 网络中的所有节点都拥有网络密钥（NetKey）。确实，拥有此共享密钥才使节点成为网络的成员。网络加密密钥和隐私密钥直接从 NetKey 派生。
>
> 拥有 NetKey 可以使节点解密消息并认证到网络层，从而可以执行诸如中继之类的网络功能。它不允许对应用程序数据进行解密。
>
> 可以将 mesh 细分为子网，每个子网都有自己的 NetKey，该 NetKey 仅由属于该子网成员的那些节点拥有。例如，这可用于隔离特定的物理区域，例如旅馆中的每个房间。
>
> 特定应用程序的应用程序数据只能由拥有正确应用程序密钥（AppKey）的节点解密。在网状网络的各个节点之间，可能有许多不同的 AppKey，但是通常，每个 AppKey 仅由所有节点中的一小部分节点拥有，即那些可以参与给定应用程序的节点。例如，灯光和电灯开关将拥有照明应用程序的 AppKey，而不拥有供暖系统的 Ap​​pKey，而暖气系统的 Ap​​pKey 仅由恒温器，散热器上的阀门等拥有。
>
> 上层传输层使用 AppKey 在将消息传递到访问控制层之前对其进行解密和身份验证。
>
> AppKey 仅与一个 NetKey 相关联。这种关联称为“密钥绑定”，表示通过拥有给定 AppKey 定义的特定应用程序只能在一个特定网络上工作，而一个网络可以托管多个独立安全的网络应用程序。
>
> 最终的密钥类型是设备密钥（DevKey）。这是一种特殊的应用程序密钥。每个节点都有一个配网器已知的唯一DevKey，其他节点则没有。DevKey 在配网过程中用于保护配网器和节点之间的通信安全。

## Node Removal, Key Refresh and Trashcan Attacks 

As described above, nodes contain various mesh security keys. Should a node become faulty and need to be disposed of, or if the owner decides to sell the node to another owner, it’s important that the device and the keys it contains cannot be used to mount an attack on the network the node was taken from. 

**A procedure for removing a node from a network is defined.** The Provisioner application is used to add the node to a **black list** and then a process called the **Key Refresh Procedure** is initiated.

The Key Refresh Procedure results in all nodes in the network, except for those which are members of the black list from being issued with new network keys, application keys and all related, derived data. In other words, the entire set of security keys which form the basis for network and application security are replaced. 

As such, the node which was removed from the network and which contains an old NetKey and an old set of AppKeys, is no longer a member of the network and poses no threat. 

> 如上所述，节点包含各种 mesh 安全密钥。如果某个节点出现故障并需要处理，或者所有者决定将该节点出售给另一个所有者，那么重要的是，不能使用该设备及其包含的密钥来对该节点原来的 mesh 网络进行攻击。
>
> 定义了从网络中删除节点的过程。配网器应用程序用于将节点添加到黑名单，然后启动“密钥刷新过程”过程。
>
> 密钥刷新过程将导致网络中的所有节点，但黑名单中的节点除外，这些节点将被发布新的网络密钥，应用程序密钥以及所有相关的派生数据。换句话说，将更新构成网络和应用程序安全性基础的整个安全密钥集。
>
> 这样，从网络中删除并包含旧的 NetKey 和旧的 AppKey 的节点不再是网络的成员，也不构成威胁。

## Privacy 

**A privacy key, derived from the NetKey is used to obfuscate network PDU header values, such as the source address.** Obfuscation ensures that casual, passive eavesdropping cannot be used to track devices and the people that use them. It also makes attacks based upon traffic analysis difficult. 

The degree of security offered in this technique is fit for purpose.

> 从 NetKey 派生的隐私密钥用于混淆网络 PDU 头中的值，例如源地址。混淆确保了不能使用随意的、被动的窃听来跟踪设备及其使用人员。这也使基于流量分析的攻击变得困难。

## Replay Attacks 

In network security, a replay attack is a technique whereby an eavesdropper intercepts and captures one or more messages and simply retransmits them later, with the goal of tricking the recipient, into carrying out something which the attacking device is not authorized to do. An example, commonly cited, is that of a car’s keyless entry system being compromised by an attacker, intercepting the authentication sequence between the car’s owner and the car, and later replaying those messages to gain entry to the car and steal it. 

**Bluetooth mesh has protection against replay attacks. The basis for this protection is the use of two network PDU fields** called the **Sequence Number** \(SEQ\) and **IV Index**, respectively. Elements increment the SEQ value every time they publish a message. A node, receiving a message from an element which contains a SEQ value less than or equal to that which was in the last valid message, will discard it, since it is likely that it relates to a replay attack. IV Index is a separate field, considered alongside SEQ. **IV Index values within messages from a given element must always be equal to or greater than the last valid message from that element.**

> 在网络安全中，重放攻击是一种技术，窃听者拦截并捕获一条或多条消息，然后简单地重新传输它们，目的是欺骗接收者，以执行未经授权的设备攻击。通常被引用的一个例子是，汽车的无钥匙进入系统受到攻击者的攻击，拦截了汽车所有者与汽车之间的身份验证序列，然后重放这些消息以进入汽车并窃取它。
>
> 蓝牙 mesh 网络具有防止重放攻击的保护。这种保护的基础是使用两个网络 PDU 字段，分别称为序列号（SEQ）和 IV 索引。元素每次发布消息时都会递增 SEQ 值。从某个节点接收到的消息中的 SEQ 值小于或等于最后一个有效消息中的 SEQ 值的节点，将丢弃它，因为它可能与重放攻击有关。IV 索引是一个单独的字段，与 SEQ 一起考虑。给定元素的消息中的 IV 索引值必须始终等于或大于该元素的最后一条有效消息。

