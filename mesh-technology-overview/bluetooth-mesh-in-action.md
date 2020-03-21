# Bluetooth Mesh in Action

## Message Publication and Delivery 

A network which uses Wi-Fi is based around a central network node called a router, and all network traffic passes through it. If the router is unavailable, the whole network becomes unavailable. 

In contrast, Bluetooth mesh uses a technique known as managed flooding to deliver messages. Messages, when published by a node, are broadcast rather than being routed directly to one or more specific nodes. All nodes receive all messages from nodes that are in direct radio range and, if configured to do so, will then relay received messages. Relaying involves broadcasting the received message again, so that other nodes, more distant from the originating node, might receive the message broadcast. 

## Multipath Delivery 

An important consequence of Bluetooth technology’s use of managed flooding is that messages arrive at their destination via multiple paths through the network. This makes for a highly reliable network and it is the primary reason for having opted to use a flooding approach rather than routing in the design of Bluetooth mesh networking. 

## Managed Flooding 

Bluetooth mesh networking leverages the strengths of the flooding approach and optimizes its operation such that it is both reliable and efficient. The measures which optimize the way flooding works in Bluetooth mesh networking are behind the use of the term “managed flooding”. Those measures are as follows: 

### Heartbeats 

Heartbeat messages are transmitted by nodes periodically. A heartbeat message indicates to other nodes in the network that the node sending the heartbeat is still active. In addition, heartbeat messages contain data which allows receiving nodes to determine how far away the sender is, in terms of the number of hops required to reach it. This knowledge can be exploited with the TTL field. 

### TTL 

TTL \(Time To Live\) is a field which all Bluetooth mesh PDUs include. It controls the maximum number of hops, over which a message is relayed. Setting the TTL allows nodes to exercise control over relaying and conserve energy, by ensuring messages are not relayed further than is required. 

Heartbeat messages allow nodes to determine what the optimum TTL value should be for each message published. 

### Message Cache 

A message cache must be implemented by all nodes. The cache contains all recently seen messages and if a message is found to be in the cache, indicating the node has seen and processed it before, it is immediately discarded. 

### Friendship 

Probably the most significant optimization mechanism in a Bluetooth mesh network is provided by the combination of Friend nodes and Low Power nodes. As described, Friend nodes provide a message store and forward service to associated Low Power nodes. This allows Low Power nodes to operate in a highly energy-efficient manner. 

## Traversing the Stack 

A node, receiving a message, passes it up the stack from the underlying Bluetooth LE stack, via the bearer layer to the network layer. 

The network layer applies various checks to decide whether or not to pass the message higher up the stack or to discard it.

In addition, PDUs have a Network ID field, which provides a fast way to determine which NetKey the message was encrypted with. If the NetKey is not recognized by the network layer on the receiving node, this indicates it does not possess the corresponding NetKey, is not a member of that subnet and so the PDU is discarded. There’s also a network message integrity check \(MIC\) field. If the MIC check fails, using the NetKey corresponding to the PDUs Network ID, then the message is discarded. 

Messages, are received by all nodes in range of the node that sent the messages but many will be quickly discarded when it becomes apparent they are not relevant to this node due to the network or subnet\(s\) it belongs to. 

The same principle is applied higher up the stack in the upper transport layer. Here though, the check is against the AppKey associated with the message, and identified by an application identifier \(AID\) field in the PDU. If the AID is unrecognized by this node, the PDU is discarded by the upper transport layer. If the transport message integrity check \(TransMIC\) fails, the message is discarded.

