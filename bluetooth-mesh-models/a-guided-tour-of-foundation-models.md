---
description: >-
  The foundation models are concerned with enabling the configuration and
  management of theBluetooth mesh network and the devices it contains.
---

# A Guided Tour of Foundation Models

## The Configuration Server and Client Models

All devices need to be configurable. Implementing the configuration server model is therefore mandatory and provides the device with the ability to be configured, typically using a smartphone application that will implement the configuration client model. 

The configuration server model contains a significant number of states that allow various aspects of a device to be configured. The device’s overall composition is held within a state called the Composition Data state. The destination address to use when publishing messages and other parameters relating to periodic message publication; the addresses subscribed to; and which, if any, of the special relay, friend, low power node, and proxy roles a device may play are all part of the configuration model’s data. 

Typically, developers only need to ensure the configuration server model is part of their device’s firmware. The configuration data comes from a configuration client application, usually at the same time the device is provisioned to equip it with security cases. However, sometimes developers explicitly perform part of the device’s configuration from within their code.

## The Health Server and Client Models

The health models are concerned with fault reporting and diagnostics. The primary element of all nodes in a Bluetooth mesh network must include the health server model. Other elements may inform the health server model of faults. A series of fault-related states, such as current fault, are defined for the health server model. Faults are represented by single octet codes. Some values in the available range are reserved for Bluetooth SIG use and others are for vendor specific codes. Table 4.2.1 in the Bluetooth Mesh Profile Specification identifies the standard fault codes defined by the Bluetooth SIG.

