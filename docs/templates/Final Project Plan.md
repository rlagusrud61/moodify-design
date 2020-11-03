# Final Project Plan report

|                  | **Names/Ids**  |
|-----------------:|:---------------|
| *Team ID:*       |      Team 08          |
| *Team Members:*  | Victor Kampen, Hyeon Kyeong Kim, Priya Naguine, Marieke Romeijn,Puru Vaish, Joep Vorage      |
| *Scrum  Mentors:*| Chakshu Gupta, Rick Fontein  |

**Project Title: Moodify**

## 1. Introduction and Background
For our project, we were inspired by how people’s mental health has been affected during the COVID-19 pandemic. Since everyone is locked in their rooms because of the pandemic, people might not get sufficient light and research shows that light can significantly improve someone’s mood and energy levels.

During this pandemic, people have been struggling and we are using this opportunity to help people. We decided to create a multi-functional lamp whose colour and brightness can change either automatically or manually. The light can also react to music, and we hope that this multi-functional lamp works as “mood lighting” and puts people in a better mood.

We called the project Moodify. The Moodify is a must-have if you want to control the atmosphere according to the mood you’re in or the mood you’d want to be in. Moodify is designed with the atmosphere of your home in mind, thereby improving your mood. 

## 2. Project Scope
The Moodify is a multi-functional lamp that has a number of features. Firstly, the Moodify features a manual light mode where the user can change the colour of the light and adjust its brightness. Secondly, the Moodify has an automatic or mood lighting mode where the colour of the light can be changed, but the brightness depends on the light intensity of the room. If the room is bright enough, the Moodify will turn itself off, or back on when needed with varying intensity. Thirdly, the Moodify has a music mode, which takes sound as input and as the rhythm of the music changes, so does the colour of the light.

All of these features are easily accessible via a custom-built web interface that accompanies the Moodify. The interface can be accessed by the internet browser on your smart device. The Moodify is connected to your smart device over Bluetooth. Turning the lamp on and off can also be done using the switch on the Moodify.

## 3. Brief description of the hardware (Raspberry Pi) and software
For this project, we used the Raspberry Pi 4 along with the breadboard, male-to-male cables, female-to-male cables, a switch, an RGB-LED strip x10, USB 5V 2A power adapter for powering the RGB-LED strip, a 330ohm resistor, a capacitor, an LDR and 1kohm resistor,USB to 3.5mm audio jack converter and a 3.5mm jack microphone. This combination of hardware was required to build the Moodify. 

The mobile phone applications “BLE Scanner” and “Light Blue” were used for testing purposes as the mockup of the client side.

The programming language involved in this project is Python, and many of its libraries are included in the application (further information included below). Also, there are bash scripts included for the configuration and installation of dependencies before running the BLE-GATT server.

The following list shows all the specifications of the software: 

1. Front-end
    * HTML
        - We implemented the front-end with a combination of regular HTML + CSS, and also used some of the W3Schools library to use some of the features (such as buttons). 
    * JS
        - There are two javascript files in the web interface: index.js and ble.js. index.js is responsible for handling some of the basic front-end interactions such as click events of radio buttons. Also, it is responsible for generating data objects (e.g. RGB hexStrings) for the Pi. The ble.js mainly handles the connection and the disconnection of the Pi to the web interface, and handles the data received by the web interface from the Pi. 
2. Bluetooth Link
    * Web-Bluetooth-API
        - This is an experimental technology hence requires the experimental flag in chrome and other web-browsers to be set to enable.
        - The documentation of this is used and can be found on their github page. For our purposes, we need only the data flow.
            1. First we have the device, the Raspberry Pi. The device is hosting the BLE-GATT-Server with many services.
            2. We get the service we need, in our case it is just the one, the MoodifyService.
            3. That Service has many characteristics:
                a. ModeCharacteristics
                b. ColourCharacteristics
                c. BrightnessCharacteristics
            4. Both services and characteristics are defined by a UUID which is used to find them in the device and the service respectively.
            5. Once the characteristics are found, we can write to those characteristics using the function calls of the Web-Bluetooth API.
            6. The updates received from the front-end are carried forward using different function calls to write to the specific characteristic which are then sent over the Bluetooth to the Raspberry Pi to the BLE-GATT-SERVER.
    * BLE-GATT-SERVER
        i. Uses the bluetooth adapter of the Raspberry Pi.
        ii. A python wrapper for controlling this adapter is used, namely pyble, which was not written by us, but is part of the official bluez examples which was modified by a user on Github for ease of access in projects.
        iii. This library is used to define our own Moodify™ Advertisement, Moodify™ Services and Moodify™ Characteristics for those Services.
        iv. Advertisement is what allows us to use a Human-Readable name when we want to connect to the Pi from our website.
        v. Services is a container for many characteristics that we would like to control in our Bluetooth Server.
        vi. Characteristics are individual elements that a user writes to from the client. These updates are carried forward to the Moodify™ Driver
3. GPIO-Programming
    * MoodifyDriver
        - Holds the current mode:
            1. Manual Mode, MoodLighting, Music Mode
            2. Receives the updates from moodify-gatt-server to switch modes and turn off all modes when needed.
            3. Calls the appropriate methods in Strip Control which reflect the updates sent by the client.
        - Threading
            1. Updates and controlling the strip happens in two different threads to ensure tasks like Music Mode work without too much delay.
            2. Controls the main loop of checking, updating and activating said modes. The updates come from the moodify-gatt-server when the user makes changes on a proper client by sending mode, colour and brightness updates.
            3. It also creates an Event Signal object to the StripControl. This strip control uses the event object in the main musicLoop.
    * StripControl
        - Uses the library ws281_x new Pixel Library
            1. Handles sending the proper bit stream to the NeoPixel LEDs we have used in our project for the colour, brightness being displayed on the pixels.
            2. We fill the RGB_LED strip with the color we specify using the library function self.fill()
            3. Then when we want to show the colour, we use self.show() command of the library to display the colour to the physical LED.
        - Depending on the call from the Moodify™ Driver it turns ON 
            1. Manual LED mode
                a. Showing the current colour using methods summarised above
            2. Turns on the Lights in Mood Lighting Mode
                a. Uses RPio.GPIO
                b. Handles reading the LDR updates when Mood Lighting mode is set.
                c. When the charging time of the capacitor is very long, it is very dark hence the lights should turn on.
                d. Else, there is sufficient light and the lights stay off.
                e. The light color is selected by the user on the Web-Interface.
            3. Toggles the Music Loop
                a. It receives an event object from Moodify™ Driver. This is used explicitly for the music mode.
                b. When the Moodify™ Driver gets the update for turning on the music mode, it sets the event object. This tells the music loop which was currently blocked, waiting for this event. This is to prevent the audio device reading even when it is not required by the user. 
                c. Turn on the Music Mode when we get a signal from Moody Driver, which begins a loop of refreshing the colour of the rgb_led_strip depending on the frequency and the amplitude of the sound received by the sound device.
                d. This loop stops when the Moodify™ Driver clears the Event object (which happens when the user turns off the music mode). This thread then waits until the event is set again, hence not wasting any CPU time.
    * SoundAnalyser
        - PyAudio
            1. Handles the aspect of reading the sound from the microphone into a buffer and then we use the data chunks returned from the buffer to analyse the data: in terms of amplitude, amplitude of peak frequency and peak frequency.
        - SciPy
            1. Creates the butter band filter for frequency filter of really high and low frequency that are not part of the songs.
            2. When PyAudio returns the data chunks, it is first passed through the butter band filter to filter the frequencies we do not need.
        - Numpy
            1. Is a base library used by both PyAudio and SciPy
            2. Implements the Hanning windowing function to smooth the frequencies before Fast Fourier Transform is applied
            3. Implements the Fast Fourier Transform function to return the frequencies of the data signal.
            4. Then we choose the frequency with the highest amplitude, and also record the amplitude.
            5. We scale the amplitude to a brightness level between 0 to 1, to simulate burst in volume in many songs for our LED.
            6. We return the Peak Frequency and Brightness to our LEDStripControl to refresh the RGB_LED Strip while in music mode.

## 4. Application Specification
Our product has three main modes, namely the manual light mode, the automatic or mood lighting mode and the music mode: 
1. In the manual light mode, the user can manually turn the light on and off by using the web interface. Furthermore, the colour of the light can be changed using an RGB colour picker on the web interface. Finally, the brightness of the light can be adjusted using a slider on the web interface. 
2. In the mood lighting mode, in which an LDR detects the light intensity of the room to decide whether the light should brighten or dim. The brightness of the lamp is adjusted according to the light intensity of the room. In the mood lighting mode, the colour can also be changed by the user in the web interface using an RGB colour picker. 
3. In music mode, the colour and the brightness of the light change according to sound input from the microphone. The colour of the light is dependent on the most prominent frequency that was detected. The brightness of the light is based on the amplitude of the sound detected. We convert the sound frequency to an RGB value to be used by the RGB LEDs by mapping the sound frequency to light frequencies and then calculating the appropriate RGB value for that light frequency. We decided on a range of sound frequencies to scan for when detecting sound so that that range could be mapped to the lightwave frequencies that correspond to RGB colours. We decided to use a range of 80-1000 Hz as this reasonably covers the frequencies that the human voice might produce during a song.

There is also a physical switch, with which we can turn the Moodify on and off.

## 5. Design (How to map application onto Hardware and Software)
- [UML diagram](https://imgur.com/TWZCyuH)
As shown in the use case diagram, the user can either use the switch or the web interface to interact with the Moodify. When the switch is used, the user can manually turn the light on and off. When the web interface is used, the user can access quite a few functionalities depending on the mode chosen. The user can switch between the three modes: manual light mode, automatic or mood lighting mode and music mode. In manual light mode, the user can change the colour of the light as well as the brightness of the light. In automatic or mood lighting mode, the user can only change the colour of the light as its brightness depends on the brightness of the room. In music mode, the user allows the microphone to take in sound input and the colour of the light and intensity is automatically adjusted according to the rhythm of the sound. 

- [Mockup for the web interface](https://imgur.com/7pDujVd)
This was the original mockup for the project but when the web interface was created, a few changes were made. Since alarm functionality overlapped with a project from a different group, and as this functionality is not of our utmost importance, we decided to not implement it for the time being. So, in the sidebar, we have radio buttons for the 3 modes. Also, there was a change to an RGB colour picker instead of selecting individual colours to allow more colours to be selected. Also, there is also a connect button (via Bluetooth to the Pi) next to the disconnect button. The switch button to turn the lights on or off was just moved to the side bar. Despite some changes from the initial design, this web interface still allows the user to achieve all the functionalities of the Moodify.

## 6. Implementation (How to implement the different parts)
During sprint 1, we decided on the idea and scope of our project. We also reflected on the different roles we were going to take upon ourselves as team members in this project.

During sprint 2, we created a mock-up to base our web interface on. We changed the scope of our project a little bit - deprioritising the wake-up light feature - to prevent overlap with other projects. Furthermore, we decided on relevant libraries and their implementations and we made ourselves familiar with said libraries.

During sprint 3, we made a working web interface based on our mock-up with features such as selecting a mode of operation, selecting the colour and the brightness of the light and connecting and disconnecting to the Raspberry Pi. The Moodify could then be connected to your smart device using Bluetooth and then the web interface could be used to control the LED. The light could be turned on and off manually or automatically based on the mode the Moodify is in and the light intensity perceived by the sensor.

During sprint 4, we implemented the RGB colour picker that the user can use to choose the colour of the light. We implemented the music mode as well. We also polishied of our web interface, for example by reorganising our buttons and renaming some of them, and removing an unnecessary scroll bar. One of the last things we implemented was the slider to adjust the light intensity.

During sprint 5, we have the final opportunity to make changes to our project. We will add a physical switch to the Moodify so that it can be turned on and off. Also, we will look at the feedback we get, and make the corresponding changes. If any changes are done during sprint 5, the testing for these changes will be done during this sprint as well.

## 7. Testing
**Manual testing**

The testing of the Moodify was very simple. Most of the functionality we wanted was controlled by the web interface, so once we were done, it was just a matter of trying all the different options available to us. We tested the Moodify more thoroughly and we tried breaking the system by switching between the various modes and toggling between other options in quick succession, as well as trying different combinations of modes and options. We have tried activating both the manual mode and the automatic mode without any success for example.

Another way of testing was done with a client side mockup called LightBlue combined with the BLE Scanner application available on Android and iOS. We used the web-interface that was in development for this testing.

We carried out the testing in 4 distinct phases, namely:
1. Stub: We used print statements and exchanged dummy values.
2. Module: Aside from the BLE, we have tested the individual modules like StripControl, SoundAnalyser, and we also tested the individual HTML components.
3. Integration testing: We tested the Functions with moodify-gatt-server over Bluetooth connection for that particular feature.
4. Full System Tests: Once the Web-Interface was done we tested the entire stack of communication from the front-end to the physical hardware on the Pi for every method and mode with different values.

Acceptance Criteria:
1. The user can connect to the Pi using the web-interface
    - By pressing the button connect, the user can perform other operations like changing the colour and the Pi responds to it (PASS)
    - There is no change after connecting to the Pi (FAIL)
2. When the user inputs a new colour the colour of Pi changes only when in Manual Light mode and Mood Lighting Mode
    - The colour does not change when the Music Mode is selected (PASS)
    - The colour changes to selected colour when Music Mode is selected (FAIL)
3. When it is dark, the lights automatically turn on to the previously selected colour in Mood Lighting mode
    - The lights do not turn on when it is dark (FAIL)
    - The lights do not turn on to the previously selected colour (FAIL)
    - The light turn on only but not to the selected colour (FAIL)
    - The lights turn on and to the selected colour (PASS)
4. When the music mode is turned on, playing music sufficiently close to the Pi causes different colours to show.
    - No colours or single colour shown while in Music Mode (FAIL)
    - Different colours shown when Music Played while in Music Mode (PASS)
5. When the Pi is disconnected, the user is alerted about the disconnection
    - An alert box is shown in the Web-Interface informing them about the disconnection (PASS)
    - No message is given to the user. (FAIL)

**Automated Testing**

There are only a few things that can be automated tested in our project due to the acute reliance on the existence of a bluetooth driver and hardware and also the existence of other physical circuit components like the RGB and the LDR. The code for this part can not be tested without the hardware. We made an automated test for the decode string functions and its assertions, by defining a pipeline which runs the decodeTest.py in our directory. This tests if the incoming string code for mode select, colour option and the brightness level.

Acceptance Criteria:
1. For the decode mode selected:
    - The string must be of length 3 and The string must only have one “1” and The string must only have numbers and The string must only have numbers in range 0 and 1 and only be integers (PASS).
    - Else (FAIL).
2. For the decode colour:
    - The string must be of the format “int,int,int” and after splitting on the separator (“,”), the string must only have numbers and the string must only have numbers in range 0 and 255 and numbers should only be integers. (PASS)
    - Else (FAIL).
3. For the decode brightness:
    - The string should only have a float number in range of 0 to 1. (PASS)
    - Else (FAIL).

## 8. Planning and definition of tasks for the individual members
**Sprint 1**
- Victor Kampen:  Scrum Assignment, Requirement Analysis Document & worked on the slides for the presentation.
- Hyeon Kyeong Kim: Scrum Assignment, Requirement Analysis Document & worked on the slides for the presentation.
- Priya Naguine: Scrum Assignment, Requirement Analysis Document & worked on the slides for the presentation.
- Marieke Romeijn:  Scrum Assignment, Requirement Analysis Document & worked on the slides for the presentation.
- Puru Vaish:  Scrum Assignment, Requirement Analysis Document & worked on the slides for the presentation. Did some reading about the Pi and how it is used and discussed possible project ideas with the group.
- Joep Vorage: Scrum Assignment, Requirement Analysis Document & worked on the slides for the presentation.

**Sprint 2**
- Victor Kampen: Software Design Document, Team Contract & worked on the slides for the presentation.
- Hyeon Kyeong Kim: Software Design Document, Team Contract, Mock-up for the web implementation & worked on the slides for the presentation.
- Priya Naguine: Software Design Document, Team Contract & worked on the slides for the presentation.
- Marieke Romeijn: Software Design Document, Team Contract & worked on the slides for the presentation. 
- Puru Vaish: Software Design Document, Team Contract & worked on the slides for the presentation. Looked up and designed the information flow through the Bluetooth connection from the raspberry pi and to the front-end web-interface, and learned the relevant technologies regarding the bluetooth domain, including BLE, GATT and bluez. A test connection was also conducted for proof of concept.
- Joep Vorage: Software Design Document, Team Contract and UML diagrams & worked on the slides for the presentation.

**Sprint 3**
- Victor Kampen: HTML, CSS and Javascript for the web interface. Victor usually made the framework, meaning that every button, slide, menu bar and other elements existed and were more or less in the right place. Also, he worked on the slides for the presentation.
- Hyeon Kyeong Kim:  HTML, CSS and Javascript for the web interface. Kim usually improved the code by making changes and implementations which made everything look much better after Victor created the framework. Kim, being the more experienced coder, also worked on some parts on her own. This was very important for the MVP as the MVP is not complete without an aesthetically pleasant user interface. Also, she worked on the slides for the presentation.
- Priya Naguine: The programming of the LED and LDR was done by Priya. She built up the circuit for the LED to turn on and off according to the light intensity of the room and also wrote the program to implement that functionality. This was crucial for the MVP as it required the use of a sensor. Also, she worked on the slides for the presentation.
- Marieke Romeijn: The programming of the LED and LDR was done by Marieke. She built up the circuit for the LED to turn on and off according to the light intensity of the room and also wrote the program to implement that functionality. A 3D sketch of the final product was made by Marieke. This was crucial for the MVP as it required the use of a sensor. Also, she worked on the slides for the presentation.
- Puru Vaish: The Web Bluetooth API on the frontend Web-Interface and BLE GATT server on the back end was implemented by Puru. This included the creation of the Moodify Service hosted on the Raspberry Pi along with the characteristics that allow data flow to take place for different data, especially of the Mode that the user has selected, and sending the message to turn the lights on and off manually. The endpoints for both the front and back end were configured and were then seamlessly connected to the works of others especially the LDR code after necessary refactoring by Puru, to suit the server so that the user could also select the automatic light mode and the design of the web-interface to the appropriate buttons and loading all scripts and connections required on startup of the web-page. With this implementation the different aspects of the project were completed and an MVP was delivered on time. Also, he worked on the slides for the presentation and report.
- Joep Vorage: Joep figured out how to incorporate the music mode and wrote the first inception of the mode fast forwarding the implementation in the next sprint. Also, he worked on the slides for the presentation.

**Sprint 4**
- Victor Kampen: Improvement of web interface, namely added the browser compatibility feature for the web interface. This was a JavaScript function that checked the bluetooth API’s compatibility with the OS, the browser type and the browser version of the user. When the user makes use of an unsupported browser type he will get the error message “This browser is not supported by our product”. If the user makes use of a browser that is compatible but is currently running in the wrong version it will either tell the user to use a newer version of the browser or, in the case of Microsoft Edge, to use an older version. Which browser version is compatible with the bluetooth API depends partly on the operating system of the user so an OS check also had to be implemented as part of this function. Victor also worked on the Sprint Retrospective Report.
- Hyeon Kyeong Kim: Improvement of the web interface, namely worked on polishing the web interface with the aim of making it look more like the mockup. She added some required buttons, sliders and colour pickers and they wrote the corresponding JavaScript code. They also fixed nuisances such as unnecessary scroll bars and the badly placed footer. Kim also worked on the Sprint Retrospective Report.
- Priya Naguine: Improvement of the web interface, namely worked on polishing the web interface with the aim of making it look more like the mockup. They added some required buttons, sliders and colour pickers and they wrote the corresponding JavaScript code. They also fixed nuisances such as unnecessary scroll bars and the badly placed footer. Priya also worked on the Sprint Retrospective Report.
- Marieke Romeijn: Improvement of web interface, namely added the browser compatibility feature for the web interface. This was a JavaScript function that checked the bluetooth API’s compatibility with the OS, the browser type and the browser version of the user. When the user makes use of an unsupported browser type he will get the error message “This browser is not supported by our product”. If the user makes use of a browser that is compatible but is currently running in the wrong version it will either tell the user to use a newer version of the browser or, in the case of Microsoft Edge, to use an older version. Which browser version is compatible with the bluetooth API depends partly on the operating system of the user so an OS check also had to be implemented as part of this function. Marieke also worked on the Sprint Retrospective Report.
- Puru Vaish: Implementation of the music mode and the integration of the RGB LED. THe single LED was replaced by Puru in our project with the digital RGB LED strip bought from Stores (at utwente) and then existing scripts, server and new code was written for controlling the RGB LED strip both in the back end and front end by Puru. A color picker was configured and added to the front-end along with a new characteristic on our BLE GATT server to select the colour of the RGB LED. THe brightness characteristic was also defined and a slider for that was added on to the user interface by Puru so that the user could choose the brightness of the RGB LED strip remotely. The necessary back-end program code was also written to both control the RGB LED strip’s colour and brightness. Necessary refactoring was also done for the changes. For the Music mode he worked with Joep to deliver a fully functional music mode, especially working on the band-pass filter, data smoothing techniques and the colour mapping. Puru also worked on the Sprint Retrospective Report. At the end of this sprint some tests were added but they were not automated until the next sprint.
- Joep Vorage: Implementation of the music mode. He bought and configured the necessary Microphone and USB to Aux adapter since the Pi could not take input from the aux it had inbuilt. He wrote and tested the code for connecting the said audio device to the python script. Along with Puru they settled on important constants that would allow for the best result and accuracy balance in the project. He was also in charge of testing the colour and the accuracy of this mode making tweaks to better it before the end of the sprint. Joep also worked on the Sprint Retrospective Report.

**Sprint 5**
- Victor Kampen: Prepare for the presentation & improve the Project Report. 
- Hyeon Kyeong Kim: Prepare for the presentation & improve the Project Report. 
- Priya Naguine: Prepare for the presentation & improve the Project Report. 
- Marieke Romeijn: Prepare for the presentation & improve the Project Report. 
- Puru Vaish: Prepare for the presentation & improve the Project Report. Given the feedback he was working with Joep on a new branch called Octave-Analysis and its branches to test different band-pass filter configurations and color mapping that would make the RGB-LED appear less confused and give more responsive and visible changes. The nature of this increment is currently unknown and they may not make it into the final sprint presentation, but it is unclear at the time of writing this report. A pipeline of automated tests were added onto Gitlab for the testing the static parts of the code.
- Joep Vorage: Prepare for the presentation & improve the Project Report. Given the feedback he was working with Puru on a new branch called Octave-Analysis and its branches to test different band-pass filter configurations and color mapping that would make the RGB-LED appear less confused and give more responsive and visible changes. The nature of this increment is currently unknown and they may not make it into the final sprint presentation, but it is unclear at the time of writing this report.

## 9. Conclusion
Despite some difficulties regarding time management, overall as a team we can proudly say that we have successfully delivered the project without major hardships. Every required assignment was completed on time, and this is due to the good communication and understanding the potential of each team member. Additionally, our project covers the entirety of the user stories and most of the functional requirements we had planned out since the beginning of the project. This indicates that thanks to our careful planning, we were able to stay away from too much deviation from the initial idea.

##  Reference

##   I. Acknowledgement
We would like to thank our TAs Rick and Chakshu for keeping us on track and always giving us feedback and keeping us on track. We would also like to thank the students that watched our Sprint Review presentations and gave us feedback. 

##   II. Reflection
Looking back on the project, we should have planned a few things differently. To begin with, we should have started working on our MVP earlier. Had we done this, we wouldn’t have had to work overtime during sprint 3 in order to meet our targets and deadlines. The approach we took, also caused the presentation for sprint 3 to be a bit unorderly due to several last minute changes. The presentation for sprint 2 was also a bit unorganised, so at the end of the day, the lesson learned is that we should have started working on our presentations earlier on.

What we also learned from the project is that it is hard for students with a non-TCS background to work the same way and do the same tasks as TCS students. The non-TCS students need to be prepared for the workload of the project, since there are a lot of deadlines so falling behind is not an option. 

One thing that stood out in this group project was the way the team members communicated with each other. There was a difference in skill-level concerning programming, but this was understood and not held against the people who were less-skilled in this area. Also, considering the fact that Priya, Joep,  Puru and Kim had worked together before, and Victor and Marieke were new, this did not affect the communication within the team. This contributed to a pleasant working environment for all of the team members.

##   III. Appendix and Glossary
Glossary:
1. BLE - BLuetooth Low Energy
2. GATT - Generic Attribute Profile
3. UUID - universally unique identifier
4. Butter Band Filter - A type of filter which takes in a data set and filters out the frequency which falls out of the high cutoff and the low cutoff.
5. High Cutoff - The highest allowable frequency
6. Low Cutoff - The lowest allowable frequency
7. HTML - HyperText Markup Language
8. JS - Javascript
9. CSS - Cascading Style Sheet
10. FFT - Fast Fourier Transform
11. Characteristics - A field in a bluetooth service which the user can read and write to.
12. Service - A Container of Characteristics in a BLE-GATT-Server
13. Hanning Windowing - A method of smoothing data so that the more prominent frequencies are highlighted while the noise is filtered.
14. Noise - Unwanted data picked from apparently random occurrences in the physical world.
15. Background Noise - A base level of noise that exists in any physical setting, different from noise from a particular component.
