# Security

## Mesh Security is Mandatory

Bluetooth LE allows the profile designer to exploit a range of different security mechanisms, from the various approaches to pairing that are possible, to individual security requirements associated with individual characteristics. Security is in fact, totally optional, and its permissible to have a device which is completely open, with no security protections or constraints in place. The device designer or manufacturer is responsible for analyzing threats and determining the security requirements and solutions for their product. 

In contrast, in Bluetooth mesh, security is mandatory. The network, individual applications and devices are all secure and this cannot be switched off or reduced in any way.

## Mesh Security Fundamentals 

The following fundamental security statements apply to all Bluetooth mesh networks: 

1. All mesh messages are encrypted and authenticated. 
2. Network security, application security and device security are addressed independently. See “Separation of Concerns” below. 
3. Security keys can be changed during the life of the mesh network via a Key Refresh procedure. 
4. Message obfuscation makes it difficult to track messages sent within the network providing a privacy mechanism to make it difficult to track nodes.
5. Mesh security protects the network against replay attacks.
6. The process by which devices are added to the mesh network to become nodes, is itself a secure process.
7. Nodes can be removed from the network securely, in a way which prevents trashcan attacks. 

## Separation of Concerns and Mesh Security Keys

At the heart of Bluetooth mesh security are three types of security keys. Between them, these keys provide security to different aspects of the mesh and achieve a critical capability in mesh security, that of “separation of concerns”. 

To understand this and appreciate its significance, consider a mesh light which can act as a relay. In its capacity as a relay, it may find itself handling messages relating to the building’s Bluetooth mesh door and window security system. A light has no business being able to access and process the details of such messages but does need to relay them to other nodes.

To deal with this potential conflict of interest, the mesh uses different security keys for securing messages at the network layer from those used to secure data relating to specific applications such as lighting, physical security, heating and so on. 

All nodes in a mesh network possess the network key \(NetKey\). Indeed, it is possession of this shared key which makes a node a member of the network. A network encryption key and a Privacy Key are derived directly from the NetKey. 

Being in possession of the NetKey allows a node to decrypt and authenticate up to the Network Layer so that network functions such as relaying, can be carried out. It does not allow application data to be decrypted. 

The network may be sub-divided into subnets and each subnet has its own NetKey, which is possessed only by those nodes which are members of that subnet. This might be used, for example, to isolate specific, physical areas, such as each room in a hotel.

Application data for a specific application can only be decrypted by nodes which possess the right application key \(AppKey\). Across the nodes in a mesh network, there may be many distinct AppKeys but typically, each AppKey will only be possessed by a small subset of the nodes, namely those of a type which can participate in a given application. For example, lights and light switches would possess the lighting application’s AppKey but not the AppKey for the heating system, which would only be possessed by thermostats, valves on radiators and so on. 

AppKeys are used by the upper transport layer to decrypt and authenticate messages before passing them up to the access layer. 

AppKeys are associated with only one NetKey. This association is termed “key binding” and means that specific applications, as defined by possession of a given AppKey, can only work on one specific network, whereas a network can host multiple, independently secure applications.

The final key type is the device key \(DevKey\). This is a special type of application key. Each node has a unique DevKey known to the Provisioner device and no other. The DevKey is used in the provisioning process to secure communication between the Provisioner and the node. 

## Node Removal, Key Refresh and Trashcan Attacks 

As described above, nodes contain various mesh security keys. Should a node become faulty and need to be disposed of, or if the owner decides to sell the node to another owner, it’s important that the device and the keys it contains cannot be used to mount an attack on the network the node was taken from. 

A procedure for removing a node from a network is defined. The Provisioner application is used to add the node to a black list and then a process called the Key Refresh Procedure is initiated.

The Key Refresh Procedure results in all nodes in the network, except for those which are members of the black list from being issued with new network keys, application keys and all related, derived data. In other words, the entire set of security keys which form the basis for network and application security are replaced. 

As such, the node which was removed from the network and which contains an old NetKey and an old set of AppKeys, is no longer a member of the network and poses no threat. 

## Privacy 

A privacy key, derived from the NetKey is used to obfuscate network PDU header values, such as the source address. Obfuscation ensures that casual, passive eavesdropping cannot be used to track devices and the people that use them. It also makes attacks based upon traffic analysis difficult. 

The degree of security offered in this technique is fit for purpose.

## Replay Attacks 

In network security, a replay attack is a technique whereby an eavesdropper intercepts and captures one or more messages and simply retransmits them later, with the goal of tricking the recipient, into carrying out something which the attacking device is not authorized to do. An example, commonly cited, is that of a car’s keyless entry system being compromised by an attacker, intercepting the authentication sequence between the car’s owner and the car, and later replaying those messages to gain entry to the car and steal it. 

Bluetooth mesh has protection against replay attacks. The basis for this protection is the use of two network PDU fields called the Sequence Number \(SEQ\) and IV Index, respectively. Elements increment the SEQ value every time they publish a message. A node, receiving a message from an element which contains a SEQ value less than or equal to that which was in the last valid message, will discard it, since it is likely that it relates to a replay attack. IV Index is a separate field, considered alongside SEQ. IV Index values within messages from a given element must always be equal to or greater than the last valid message from that element.

