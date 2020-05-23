# Lighting

## 1 Lighting states

### 1.1 Introduction

There are different types of light sources with different capabilities. Accordingly, there are different ways to express a state of a light.

### 1.2 Light Lightness state

The Light Lightness state is a composite state that includes the Light Lightness Linear, the Light Lightness Actual, the Light Lightness Last, and the Light Lightness Default states.

#### 1.2.1 Light Lightness Linear

The Light Lightness Linear state represents the lightness of a light on a linear scale. The state is bound to the Light Lightness Actual state. The values for the state are defined in the following table.

#### 1.2.2 Light Lightness Actual

The Light Lightness Actual state represents the lightness of a light on a perceptually uniform lightness scale \[6\]. The state is bound to the Generic Level state and the Generic OnOff state. The values for the state are defined in the following table.

The perceived lightness of a light \(L\) is the square root of the measured light intensity \(Y\): 

Where L is the perceived lightness and Y is the measured light intensity \(from 0 to 65535\).

