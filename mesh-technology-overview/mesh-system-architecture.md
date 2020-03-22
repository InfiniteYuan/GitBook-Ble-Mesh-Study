---
description: >-
  In this section, we’ll take a closer look at the Bluetooth mesh architecture,
  its layers and their respectivere sponsibilities.
---

# Mesh System Architecture

## Overview

![The Bluetooth mesh architecture](../.gitbook/assets/mesh-system-architecture.png)

At the bottom of the mesh architecture stack, we have a layer entitled Bluetooth LE. In fact, this is more than just a single layer of the mesh architecture, it’s the **full Bluetooth LE stack**, which is required to **provide fundamental wireless communications capabilities** which are leveraged by the mesh architecture which sits on top of it. It should be clear that the mesh system is dependent upon the availability of a Bluetooth LE stack. 

We’ll now review each layer of the mesh architecture, working our way up from the bottom layer.

## Bearer Layer 

Mesh messages require an underlying communications system for their transmission and receipt. **The bearer layer defines how mesh PDUs will be handled by a given communications system.** At this time, two bearers are defined and these are called the **Advertising Bearer** and the **GATT Bearer**. 

The **Advertising Bearer** leverages Bluetooth LE’s GAP advertising and scanning features to convey and receive mesh PDUs. 

The **GATT Bearer** allows a device which does not support the Advertising Bearer to communicate indirectly with nodes of a mesh network which do, using a protocol known as the **Proxy Protocol**. The Proxy Protocol is encapsulated within GATT operations involving specially defined GATT characteristics. **A mesh Proxy node** implements these GATT characteristics and supports the GATT bearer as well as the Advertising Bearer so that it can convert and relay messages between the two types of bearer.

## Network Layer 

**The network layer defines the various message address types and a network message format which allows transport layer PDUs to be transported by the bearer layer.** 

It can support multiple bearers, each of which may have multiple network interfaces, including the **local interface** which is used for communication between elements that are part of the same node. 

**The network layer determines which network interface\(s\) to output messages over.** An input filter is applied to messages arriving from the bearer layer, to determine whether or not they should be delivered to the network layer for further processing. Output messages are subject to an output filter to control whether or not they are dropped or delivered to the bearer layer. 

**The Relay and Proxy features may be implemented by the network layer.** 

## Lower Transport Layer 

**The lower transport layer takes PDUs from the upper transport layer and sends them to the lower transport layer on a peer device.** Where required, it performs **segmentation** and **reassembly** of PDUs. For longer packets, which will not fit into a single Transport PDU, the lower transport layer will perform segmentation, splitting the PDU into multiple Transport PDUs. The receiving lower transport layer on the other device, will reassemble the segments into a single upper transport layer PDU and pass this up the stack.

## Upper Transport Layer 

**The upper transport layer is responsible for the encryption, decryption and authentication of application data passing to and from the access layer.** It also has responsibility for **transport control messages**, which are internally generated and sent between the upper transport layers on different peer nodes. These include messages related to **friendship** and **heartbeats**. 

## Access Layer 

**The access layer is responsible for defining how applications can make use of the upper transport layer.** This includes:

* Defining the **format** of application data. 
* **Defining and controlling the encryption and decryption process** which is performed in the upper transport layer. 
* **Verifying that data** received from the upper transport layer is for the right network and application, before forwarding the data up the stack.

## Foundation Models Layer 

**The foundation model layer is responsible for the implementation of those models concerned with the configuration and management of a mesh network.** 

## Models Layer 

**The model layer is concerned with the implementation of Models and as such, the implementation of behaviors, messages, states, state bindings and so on, as defined in one or more model specifications.**

