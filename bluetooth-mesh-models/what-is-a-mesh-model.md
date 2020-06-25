# What is a Mesh Model?

A Model defines a set of States, State Transitions, State Bindings, Messages and other associated behaviors. An Element within a Node must support one or more models and it is the model or models that define the functionality that an Element has. There are a number of models that are defined by the Bluetooth SIG and many of them are deliberately positioned as “generic” models, having potential utility within a wide range of device types.

Essentially, models are specifications for standard software components that, when included in a product, determine what it can do as a mesh device. Models are self-contained components and products will incorporate several of them. Collectively, from a network’s point of view, models make the device what it is.

## State

Models contain states. States are data items that indicate the condition of the device, such as on/off or high/low. States may be simple, containing only a single value, or composite, containing multiple fields, similar to a struct in programming languages like C. 

In some cases, there are relationships defined between state items. These relationships are called state bindings. A state binding indicates that if one of the states in the relationship changes, then the other one needs to have its value recalculated. Sometimes state bindings are conditional and may be enabled or disabled by some other state. Developers must implement the required logic for any state bindings that are defined for the models they are using and ensure that logic is executed whenever required. 

Conversely, where state bindings are not explicitly defined in the Bluetooth Mesh Model Specification, states must act independently. For example, if the generic on/off state indicates that a device is currently off, increasing the generic level state should have no user-discernible effect. Switching the device on by setting the generic on/off state to 1 should not only switch the device on, but it should begin functioning at the level that has been set. This can be readily understood if you consider a rotary dimmer switch that is rotated to change the level of the lights in the room but can also be pressed to switch them on or off. You can rotate the control when the lights are off and nothing will appear to happen, but if you then press the switch, with it in the same rotated position, the lights will come on at the selected level of brightness.

