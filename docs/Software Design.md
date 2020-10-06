# Software Design Document Template

|                 | **Names/Ids**  |
|----------------:|:---------------|
| *Team members:* |   Victor Kampen, Hyeon Kyeong Kim, Priya Naguine, Marieke Romeijn, Puru Vaish, Joep Vorage             |
| *Team ID:*      |  Team 08             |

**Instructions:**
* Make a document by including all the suggested sections.
* The document should be of a maximum of 5 and a minimum of 3 pages.
 

## Introduction

**(The overview of the entire document is mentioned.)**

This document includes the user and functional requirements for the Moodify. It then includes the tasks that need to be completed in order to achieve the specific project goals. After that, we prioritize the requirements that need to be fulfilled in order to have an MVP.

## Product User Interface

**(Create illustrations and design of your product and explain the tools used for the design. Include UML diagrams of your application for better understanding.)**

* Wireframing Tools 
- Used Clip Studio to create the illustration of an overall view of our web interface
- [Wireframing](https://imgur.com/a/gB5310l)
* UML diagrams
- Used Visual Paradigm
- [UML Diagram](https://imgur.com/6AbwJSY)

## Requirements/System Overview 

**(Overall functionality of the system is explained with user requirements and functional requirements. Describe the scenario when your system can fail.)**

### User and functional requirements:
* The user should be able to turn the light on/off with a switch, button, or something similar.
* The user should be able to choose the color of the light..
* There should be at least 5 options for the colours of the light
* The standard colour (one of the 5) is white.
* The user should be able to manually override the brightness of the lamp.
* The user should be able to select out of the following modes that the Moodify has
    1. Regular colour light: the Moodify acts like a normal lamp. The user can choose the colour
    2. Wake up light: the Moodify will wake up the user by turning itself on and increasing its brightness.
    3.  Music mode: the Moodify will have flashing lights that interact with the music
* The user should be able to turn off the light remotely (using an app or web interface)
* The user should be able to set the time at which the light is turned on in wake-up light mode.
* The user should be able to turn off the lamp remotely.
* The user should be able to configure the settings of the different modes remotely.
* The user should be able to use the Moodify as normal, i.e. non-smart, light source.

### Scenarios where the system can fail:
* The Moodify works only for a limited frequency and depends on the timbre. High distortion will lead to queasy lighting and may not respond in an accurate way.
* The Moodify can not yet upload music straight to the Raspberry Pi.
* The Moodify works with stationary power sources, or a power bank, it does not have a Lithium-ion battery or alike to charge on the go.
* In a concrete room, the sound bounces off, and echoes will degrade performance and accuracy of the lights, so it does not work well


## Milestones

**(This is basically to understand the project scope. It should be described according to the number of jobs required for your application to the final deliverable.)**

Below is a short list of the specific project goals:
* Moodify changes its light intensity according to the light intensity of the room it is in.
* The user can control whether the light is on or off. In the case that it is on, the intensity of the light and the colour can be changed.
* Moodify changes the colour and intensity based on the rhythm of the sound input.
* The Moodify can also be used as an alarm using light instead of sound. The light will gradually increase in intensity.
* There will be a web interface that controls the functionality of the Moodify.
* The Moodify allows for various modes from which one is selected.

In order to fulfill the project goals, the following tasks need to be done:
* We have to configure the Raspberry Pi in order to use it.
* We have to connect the web application with the Raspberry Pi.
* We need to configure the Raspberry Pi to allow sound input.
* We need to configure the Raspberry Pi to show lights.
* The Raspberry Pi needs to be configured with light and sound sensors.
* We need to have a completely functioning web application that controls the light.
* We need to write a program to combine everything in order to complete the project.


## Requirement Priorities

**(You are required to prioritize the functionalities to build your Minimal Viable Product. It should be stated clearly.)**

* The Moodify should take in sound input from the sound sensor.
* The Moodify should change the colour of the light according to the sound input.
* The Moodify should change the light intensity according to the sound input.


## Conclusion

**(You can give the concluding statements of your document.)**

Overall, our software should strictly follow the functional requirements that we have set as priorities. However, for now, as we are still in the phase of discovering new possibilities and constraints (either technical or practical), there might be some changes to be made in the scope of the project: for example, as we designed our project we have realized that our project is only compatible for Chrome, Opera, Microsoft Edge, and Samsung Internet (on mobile). These kinds of changes shall be later on added to our limitations. 

## Reference

[Article about Web Bluetooth](https://www.smashingmagazine.com/2019/02/introduction-to-webbluetooth/)