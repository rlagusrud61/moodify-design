# Requirement Analysis Document Template
 
|                 | **Names/Ids**  |
|----------------:|:---------------|
| *Team members:* | <ul><li>s2112019</li><li>s2172968</li><li>s2236141</li><li>s2315653</li><li>s2546612</li><li>s1992988</li></ul>               |
| *Team ID:*      |    Team 8            |


**Instructions:**
* Make a document by including all the suggested sections.
* The document should be of a maximum of 4 pages.

## Introduction
We all know about smart lights. We all know about light bulbs that can change colour. We all know about wake-up lights and we all know about timers that control the time when a lamp turns on and off, but nobody knows about a light that combines all of these functionalities. So, we present to you our project! Buying all these devices separately is a thing of the past, for we present to you a Raspberry Pi powered light called the Moodify. The Moodify is a must-have if you want to control the atmosphere according to the mood you’re in or the mood you’d want to be in. Moodify is designed with the atmosphere of your home in mind, thereby improving your mood. We designed Moodify with not one, not two, but many kinds of usage in mind. 
### Motivation
- The Moodify can be used for many different purposes.
- The Moodify turns on automatically when it gets darker, so you do not have to turn on the light when you are working. 
- The lights of the Moodify will change according to music, which can be fun during a party. 
- The Moodify will wake you up in a very calm way. It wakes your body up in a natural way, thereby improving both your mood when you wake up and your feeling of well-restedness.
- With its adaptive brightness, the room will always be bright enough but only at the hours that you want it to, therefore, keeping electricity usage low.
- The colors can be changed manually using the web interface according to your mood. 
- The Moodify will find a place in your home or office and provide you with the exact atmosphere you want and need.

###  Scope
The following are the dates and the deliverables for the corresponding dates:
- October 6, 2020: software design document and team contract
- October 9, 2020: mockup and presentation
- October 16, 2020: all materials used to demonstrate the product increment and an MVP demo 
- October 30, 2020: all materials used to demonstrate the product increment and an MVP demo 
- November 4, 2020: all materials used to demonstrate the final product and a product demo


Below is a short list of the specific project goals:
- Moodify changes its light intensity according to the light intensity of the room it is in.
- The user can control whether the light is on or off. In the case that it is on, the intensity of the light and the colour can be changed.
- Moodify changes the colour and intensity based on the rhythm of the sound input.
- The Moodify can also be used as an alarm using light instead of sound. The light will gradually increase in intensity.
- There will be a web interface that controls the functionality of the Moodify.
- The Moodify allows for various modes from which one is selected.

### Limitations of the current system (if any)
- The Moodify works only for a limited frequency and depends on the timbre. High distortion will lead to queasy lighting.
- The Moodify can not yet upload music straight to the Raspberry Pi.
- The current method of changing mode is unintuitive, this will be improved in future generations.
- The Moodify works with stationary power sources.

### Objectives
- The Moodify can be used as an alarm that will wake you up with light, not sound. 
- During the day, it can be used as both a regular light and a smart/adaptive light. If the white light is not to your liking, you can change the colour of the Moodify. 
- When you play music, it is even possible to make the music interact with the colours of the light bulb.

### Definitions and Abbreviations 
- Lamp: A tool which is used to shine light on a desk or light up a room.
- Frequency: The number of times a certain action is done per unit of time.
- Timbre: the number of distinct frequencies that make up a sound.
- MFL: multi-functional lamp.


### Overview of the selected application
- There is a light to sound visualiser.
- The Moodify has a study light mode, which means that when it gets dark, the light automatically turns on. 
- The Moodify has a built-in wake up light.
- The Moodify can also be controlled by the user manually via a web interface. 

## Product Requirements
- As a user, I would like the flashing light to portray the rhythm of the music, in party mode. 
- As a user, I would like the light intensity of the lamp to correspond to the light levels in the room, in light mode. 
- As a user, I would like to turn off the light using a switch.
- As a user, I would like to turn off the light remotely, using an app or web interface.
- As a user, I would like to switch the modes for the light.
- As a user, I would like to set the time at which the light is turned on in wake up light mode.
- As a user, I would like to adjust the colours of the lights.
- As a user, I would like to automatically change intensity as daylight levels change.
- As a user, I would like to toggle the color of the light (warm or cold), for night.
- As a user, I would like to manually override the brightness of the lamp.
- As a user, I would like to set an alarm on the clock.
- As a user, I would like to control the loudness of the alarm sound.
- As a user, I would like to change the alarm tone for the alarm.
- As a user, I want at least 3 different colors in the lamp.
- As a user, I would like the lamp to not blink faster than a frequency set by me.
- As a user, I would like to turn off the lamp remotely.
- As a user, I would like to configure the settings of the different modes remotely.

### Functional requirements
- The Moodify should have a way of knowing which mode is currently selected.
- There should be a maximum allowable flashing rate.

### Nonfunctional requirements
- It should not give the user photo epilepsy.
- It shouldn’t overheat. So, when it crosses a threshold, it automatically shuts down.
- It should be remotely accessible, through the web interface.

## Conclusion
- What the Moodify does is basically provide light which changes intensity according to the time of day and sound input. 
- The user can also control the light and use it as an alarm. There will be a web interface which can be used to control the device.

## Reference
- Music controlled LED lights: [LED Lights Connected to Music : 4 Steps (with Pictures)](https://www.instructables.com/id/LED-Lights-Connected-to-Music/)
- Wake Up light: [Home Automation Tom's HAT - Wakeup Light Alarm](https://developpa.io/home-automation-wakeup-light-alarm/)
- Remote Controlling: [A sunrise-alarm-clock-web-service using a Raspberry-Pi](https://boddy.im/sunrise-alarm-clock-web-service.html)

