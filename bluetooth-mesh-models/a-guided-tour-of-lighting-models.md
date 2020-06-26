# A Guided Tour of Lighting Models

Lighting can be surprisingly sophisticated and therefore needs specialized Bluetooth mesh models to meet its sometimes complex requirements. The Bluetooth mesh lighting models allow control over the on/off state of lights, their lightness, color temperature, and their color \(using various color spaces\). Importantly, they also provide a highly sophisticated software-based lighting controller that can enable smart lighting automation scenarios. As Figure 10 shows, there are 16 lighting models related to 5 distinct aspects of lighting. 

Before beginning the guided tour of the models, consider the nature of lights and the various ways they can be controlled.

## Lighting Overview

### Controlling Lights 

Lights are often controlled manually by pressing buttons, turning knobs, or pushing sliders. But they can also be controlled by sensors, indicating to the lights that there is someone in the room or that the ambient light level has become low because it is getting later in the day or because a cloud has obscured the sun. Lights can be controlled by timers too. 

The generic onoff and generic level models detailed in section 4 could be used to control some of the basic attributes of a light, but people perceive lighting conditions in more complex ways, with brightness or lightness perceived according to a non-linear scale. 

Lights have more attributes than their on/off state or their lightness that we might wish to control. Some lights can have their color controlled, and there are a number of ways of modelling color in lights. 

### Smart Lighting

Smart buildings require smart lighting. Smart lighting can be controlled by manual actions taken by building occupants, but, more importantly, a smart lighting system is informed by sensors and uses control algorithms to achieve self-optimizing behaviors that make the system efficient, cost effective, and pleasing to the people that use the building. The Bluetooth mesh lighting models include a particularly special set of models, such as the Light LC models that provide sophisticated, automated control of lights.

## Lighting Concepts 

To appreciate the lighting models, it helps to understand certain concepts from the world of lighting. The key ones are as follows: 

### Color Temperature 

The color temperature of a light source is what leads people to describe colors as either cool or warm. It has a more scientific definition that relates to the temperature of the light radiated by the object, measured in Kelvin. Surprisingly, lower color temperatures are those we describe as warm and higher temperatures as cool. In commercial lighting applications, warmer color temperatures are often used to promote relaxation and cooler temperatures to enhance the concentration of occupants working in the room. 

### Color-Tunable Light 

Color-tunable light \(CTL\) is a capability of some lights that allows color temperature to be controlled via two dimensions: lightness and color temperature. 

### Hue 

Colored light has a number of properties, of which hue is one of the main ones. Typically, hue measures the angular position of a color in a color wheel. 

### Lightness 

Lightness is the term used to refer to the perception of brightness. 

### Saturation 

Saturation is another property of colored light and measures the ratio of an object’s color to its lightness. A given color with a high lightness is said to be less saturated than the same color with low lightness. 

### Color Models 

A color model, not to be confused with a Bluetooth mesh model, is a mathematical way of representing colors. There are several color models in popular use, each with its own strengths and weaknesses. 

**HSL** \(hue, saturation, lightness\) represents colors using a cylindrical representation. The angular position in the circular cross section of the cylinder represents the hue, the distance from the center of this circle represents the saturation, and the distance from one end of the cylinder represents the lightness, with one end representing black and the other white.

The **RGB** \(red, green, blue\) color model is an additive model where given levels of red, green, and blue light are mixed to produce a color that people can perceive. 

The **CIE1931** color space defines the mathematical relationships between wavelengths of light and perceived colors in vision. Just like RGB and HSL, colors in this model are defined by three values: x, y, and Y. x and y are coordinates of the color on a color chart, and Y measures the luminous intensity. CIE1931 is especially popular in professional lighting applications. Each color model has an associated color space that is a set of colors that the model allows to be reproduced.

