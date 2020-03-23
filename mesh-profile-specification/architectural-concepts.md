---
description: >-
  The mesh networking architecture uses several different concepts: states,
  messages, bindings, elements,addressing, models, publish-subscribe, mesh keys,
  and associations.
---

# Architectural concepts

## States

**A state is a value representing a condition of an element.** 

An element **exposing** a state is referred to as a **server**. For example, the simplest server is a Generic OnOff Server, representing that it is either on or off. 

An element **accessing** a state is referred to as a **client**. For example, the simplest client is a Generic OnOff Client \(a binary switch\) that is able to control a Generic OnOff Server via messages defined by the Generic OnOff Model. 

States that are composed of two of more values are known as **composite states**. For example, a color- changing lamp can control color hue separately from color saturation and brightness.

## Bound states 

**When a state is bound to another state, a change in one results in a change in the other.** Bound states may be from different models in one or more elements. For example, a common type of binding is between a Level state and an OnOff state: changing the Level to 0 changes the bound OnOff state to Off and changing the Level to a non-zero value changes the bound OnOff state to On.

## Messages 

**All communication within a mesh network is accomplished by sending messages.** **Messages operate on states.** For each state, there is a defined set of messages that a server supports and a client may use to request a value of a state or to change a state. A server may also transmit unsolicited messages carrying information about states and/or changing states.

**A message is defined as having an opcode, associated parameters, and behavior.** An opcode may be a single octet \(for special messages that require maximum possible payload for parameters\), 2 octets \(for standard messages\), or 3 octets \(for vendor-specific messages\). 

A total message size, including an opcode, is determined by the underlying transport layer, which may use a **Segmentation and Reassembly** \(SAR\) mechanism. To maximize performance and avoid the overhead of SAR, a design goal is to fit messages in a single segment. **The transport layer provides up to 11 octets for a non-segmented message**, leaving up to 10 octets that are available for parameters when using a 1-octet opcode, up to 9 octets available for parameters when using a 2-octet opcode, and up to 8 octets available for parameters when using a vendor-specific 3-octet opcode. 

**The transport layer provides a mechanism of SAR capable of transporting up to 32 segments.** **The maximum message size when using the SAR is 384 octets**. This means \(excluding an Application MIC\) up to 379 octets are available for parameters when using a 1-octet opcode, up to 378 octets are available for parameters when using a 2-octet opcode, and up to 377 octets are available for parameters when using a vendor-specific 3-octet opcode. 

**SAR effectively does not impose any extra overhead on the access layer payload per segment:** a 10-octet message is transported as an unsegmented message, and a 20-octet message is transported as a segmented message that uses two segments. 

Message definitions contain tables of parameters. In a message payload, parameters follow an opcode, and parameter offsets are in octets unless otherwise specified.

**Messages are defined as acknowledged or unacknowledged.** An acknowledged message requires a response whereas an unacknowledged message does not require a response.

## Elements

**An element is an addressable entity within a node.** Each node has at least one element, the **primary element**, and may have one or more additional **secondary elements**. The number and structure of elements is static and **does not change** throughout the lifetime of a node \(that is, as long as the node is part of a network\). 

The **primary element** is addressed using the **first unicast address** assigned to the node during provisioning. Each additional **secondary element** is addressed using the **subsequent addresses**. These unicast element addresses allow nodes to identify which element within a node is transmitting or receiving a message.

If the number and structure of elements **changes**, for example due to a firmware update, the node must be **reprovisioned**. The **Node Removal procedure** is used when a firmware update is performed that **changes the number or structure of elements**.

**Messages are dispatched within models based on opcodes and element addresses.**

**An element is not allowed to contain multiple instances of models that use the same message in the same way** \(for example, receive an “On” message\). When multiple models within the same element use the same message, the models are said to “overlap.” To implement multiple instances of overlapping models within a single node \(for example, to control multiple light fixtures that can be turned on and off\), **the node is required to contain multiple elements.**

For example, a light fixture may have two lamps, each implementing an instance of the Light Lightness Server model and an instance of the Generic Power OnOff Server model. This requires that the node contain two elements, one for each lamp. When it receives an "On" message, the node uses the unicast address of the element to identify which instance of the Generic Power OnOff Server model the message is addressed to. 

In another example, a dual-socket power strip contains two independent energy measurement sensors that can measure power consumed by an appliance connected to a socket. **This would require that the node have two Sensor Data states, each in a separate element**. The first element, the primary element, would be identified using the unicast address for the node and would include a state for the first energy sensor as well as states representing the configuration of the node. The second element, a secondary element, would be identified using a unicast element address and would include the state for the second energy sensor. 

**Each element has a GATT Bluetooth Namespace Descriptor value** that helps identify which part of the node this element represents. These namespace descriptor values use the same definitions as GATT. For example, the elements of the temperature sensor would use the values “inside” and “outside.”

## Addresses

**An address may be a unicast address, a virtual address, or a group address**. **There is also a special value to represent an unassigned address that is not used in messages.** 

**A unicast address** is allocated to an element and always represents a single element of a node. There are **32767** unicast addresses per mesh network. 

**A virtual address is a multicast address** and can represent multiple elements on one or more nodes. **Each virtual address logically represents a Label UUID**, which is a 128-bit value that does not have to be managed centrally. Each message sent to a Label UUID includes the full Label UUID in the message integrity check value that is used to authenticate the message. To reduce the overhead of checking every known Label UUID, a hash of the Label UUID is used. **There are 16384 hash values, each of which codifies a set of virtual addresses. While there are only 16384 hash values used in a virtual address, each hash value can represent millions of possible Label UUIDs;** therefore, the number of virtual addresses is considered very large. 

**A group address is a multicast address** and can represent multiple elements on one or more nodes. There are **16384** group addresses per mesh network. There are a set of fixed group addresses that are used to address a subset of all primary elements of nodes based on the functionality of those nodes. All other group addresses are known as dynamically assigned group addresses. **There are 256 fixed group addresses and 16128 dynamically assigned group addresses.**

## Models 

**A model defines the basic functionality of a node**. A node may include multiple models. A model defines the required states, the messages that act upon those states, and any associated behavior.

**A mesh application is specified using a client-server architecture communicating with a publish-subscribe paradigm.** Due to the nature of mesh networks and the recognition that the configuration of behavior is ****performed by a Configuration Client, an application is not defined in a single end-to-end specification such as a profile. Instead, an application is defined in a client model, a server model, and a control model. 

This specification defines three types of model: server models, client models, and control models: 

* **Server model**: A server model is composed of one or more states spanning one or more elements. The server model defines a set of mandatory messages that it can transmit or receive, the behavior required of the element when it transmits and receives such messages, and any additional behavior that occurs after messages are transmitted or received. 
* **Client model**: A client model defines a set of messages \(both mandatory and optional\) that a client uses to request, change, or consume corresponding server states, as defined by a server model. The client model does not have state. 
* **Control model**: A control model may contain client model functionality to communicate with other server models and server model functionality to communicate with other client models. A control model may also contain control logic, which is a set of rules and behaviors that coordinate the interactions between other models that the control model connects to.

## Publish-subscribe and message exchange

Publication and subscription of data within the mesh network is described as using a publish-subscribe paradigm. Nodes that generate messages publish the messages to a unicast address, group address, or virtual address. Nodes that are interested in receiving the messages will subscribe to these addresses. 

Generated messages are sent to destination mesh addresses that can be unicast, pre-configured group addresses, or virtual addresses. Messages can be sent as replies to other messages or can be unsolicited messages. When an instance of a model is sending a reply message, it uses the incoming message originator’s source address as the destination address. When an instance of a model is sending unsolicited messages, it uses a model publish address as the destination address. Each instance of a model within a node has a single publish address. 

On the receiving side, each instance of a model within a node can subscribe to one or more group addresses or virtual addresses. Whenever a message that is addressed to a group address or a virtual

