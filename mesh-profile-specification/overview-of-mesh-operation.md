# Overview of mesh operation

The mesh network operation defined by this specification is designed to: 

* enable messages to be **sent from one element to one or more elements;**
* allow messages to be **relayed** via other nodes to extend the range of communication;
* **secure messages against known security attacks**, including eavesdropping attacks, man-in-the-middle attacks, replay attacks, trash-can attacks, brute-force key attacks, and possible additional security attacks not documented here;
* **work on existing devices** in the market today;
* **deliver messages in a timely manner;**
* continue to **work** **when** one or more devices are **moved** or stop **operating**; and
* have **built-in forward compatibility** to support future versions of the Mesh Profile specification.

This specification defines a managed-flood-based mesh network, which uses **broadcast** **channels** to **transmit** **messages** so that other nodes can receive messages and relay these messages; thus extending the range of the original message. Any device in a managed-flood mesh network can **send a message at any time** as long as there is a sufficient density of devices that are listening and relaying messages. Enhancements to add **routing functionality** and define **a routing-based mesh network** may be considered for a **future** **revision** of this specification.

There are a number of methods used by this specification to **restrict the unlimited relaying of messages** in a managed-flood mesh network. The two main methods used are the **network message cache method** and the **time to live** **method**.

The **network message cache** is designed to prevent devices from relaying previously received messages by adding all messages to a cached list. When a message is received, it is checked against the list and ignored if already present. If not already received, then it is added to the cache so that it can be ignored in the future. To prevent this list from becoming too long, **the number of messages that are cached is limited by implementation.**

Each message includes a **Time to Live \(TTL\)** value that limits the number of times a message can be relayed. Each time a message is received and then relayed \(up to a maximum of **126** times\) by a device, the TTL value is decremented by 1.

## Network and subnets

A mesh network consists of nodes sharing four common resources:

* **network addresses** used to identify source and destination of messages;
* **network keys** used to secure and authenticate messages at the network layer;
* **application keys** used to secure and authenticate messages at the access layer;
* and • an **IV Index** used to extend the lifetime of the network

A network can have one or more subnets that facilitate ”area” isolation \(e.g., isolated hotel room subnets within a hotel network\). A subnet is a group of nodes that can communicate with each other at a network layer because they share a network key. A node may belong to one or more subnets by knowing one or more network keys. At the time of provisioning, a device is provisioned to one subnet and may be added to more subnets using the **Configuration Model**. 

There is one special subnet called the **primary subnet**, which is based on the primary NetKey. Nodes on the primary subnet participate in the **IV Update procedure** , and propagate IV updates to other subnets, **while nodes on other subnets only propagate the IV Index updates to those subnets.** 

The network resources are **managed** by a node that implements the **Configuration Client model**, known as the **Configuration Client**, \(typically a smart phone or other mobile computing device\) and are **allocated** to nodes at the time of configuration using the **Configuration Server model**. In particular, a **Provisioner** manages allocation of addresses to make sure no duplicate unicast addresses are allocated, whereas a **Configuration Client generates and distributes network and application keys** and makes sure that devices that need to communicate with each other share proper keys for both network and access layers. The Configuration Client also **knows device keys**, which are used to **secure communication with each individual node, including distributing updated network and application keys.**

## Devices and nodes 

A device that is not a member of a mesh network is known as an **unprovisioned device**. A device that is a member of a mesh network is known as a **node**. A **Provisioner** is used to **manage the transitions between an unprovisioned device and a node**. 

An unprovisioned device cannot send or receive mesh messages; however, it advertises its presence to Provisioners. A **Provisioner** will invite an unprovisioned device into a mesh network after it has been **authenticated**, converting the unprovisioned device into a node.

A node can send or receive mesh messages and is managed by a **Configuration Client**, that may also be the same device as the Provisioner, over the mesh network to configure how the node communicates with other nodes. A **Configuration Client** can **remove** a node from a mesh network, which reverts it back to an unprovisioned device. 

A **device** may **support** **multiple instances of a node** by offering itself to be **provisioned to another mesh network** after already being **provisioned to a mesh network**. Each instance of a mesh network is determined by addresses and a device key obtained by the device during provisioning.

## Adding devices to a mesh network 

**Devices are added to a mesh network by a Provisioner**, at which point they become nodes. The provisioning of devices into a mesh network differs from the point-to-point bonding and pairing that is typically used in Bluetooth wireless technology. Provisioning of devices is enabled using either a simple **advertising bearer** or a **point-to-point GATT-based bearer**. A single provisioning protocol is used over both bearers. Provisioning over an advertising-based bearer is implemented by all devices. Provisioning over a GATT-based bearer allows devices such as legacy phones \(i.e., devices that do not support provisioning over an advertising bearer natively\) to be Provisioners. 

To assist with provisioning of multiple devices, a device has an **attention timer** that can be set by a Provisioner. When set to a non-zero value, the device identifies itself using any means it can. For example, the device may flash a light, make a sound, or vibrate. **When the attention timer expires, the device stops identifying itself.** This allows a **Provisioner to send a single message to a device to cause it to identify itself and the device automatically stops identifying itself after a given time.** 

**The protocol to run over these two bearers is a derivative of the Security Manager protocol of v4.2** of the Bluetooth Core Specification to introduce the ability to authenticate devices that have a very limited user interface, such as a light or a switch. The Security Manager protocol requires a reliable bearer, something that cannot be guaranteed by the advertising provisioning bearer; therefore the protocol used in this specification is designed to enable reliable delivery of messages independent of the bearer. The similarity to the Security Manager protocol enables significant reuse of existing code on devices that have implemented such functionality.

## Communications support 

Many current devices are unable to advertise or comprehend mesh messages without being updated. To enable these devices to communicate with a node in a mesh network without the need for an operating system update or similar hardware/software update, the specification **enables the use of GATT connectivity for all existing devices.**

## Low power support 

**The features within this specification enable many devices in the mesh network to be battery-powered or to use techniques such as energy harvesting.** Such devices may be constrained in how they can function as a part of a mesh network \(e.g., devices that only send data when interacted with\). This specification does not require devices to coordinate transmissions, make connections, or restart security on every connection; thus facilitating low power operation. Devices needing low power support can associate themselves with an always-on device that stores and relays messages on their behalf, using the concept known as **Friendship**. However, devices that relay messages will receive messages as well as forward messages a majority of the time and are likely to use significantly more power than could be provided by typical small batteries or capacitors.

