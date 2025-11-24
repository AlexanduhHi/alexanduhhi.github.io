---
layout: post
title: Digital Audio Synthesizer
description:  My senior design project for my Bachelor's degree, which is a Digital Audio Synthesizer that utilizes digital signal processing (DSP) techniques and extensive programming in the STM32 development environment. Group project with Philip Schremp, a fellow EE & Cal Poly alum.
skills: 
- STM32Cube IDE
- STM32F746NG-DISCO
- C/C++
- Digital Signal Processing
- 3D Printing
- Breadboarding
- Soldering
- Verification (using Lab Equipment)
- Oscilloscopes & DSOs
- Documentation & Technical Writing
- Project Planning (Gantt Charts, Functional Decompositions, etc.)
- System Integration
main-image: /DAS_on.jpg 
---

The device takes user-controlled parameters (adjusted using encoders/buttons) and musical-instrument digital interface (MIDI) signals and uses them to synthesize electronic sounds. The goal was to emulate the standard analog subtractive synthesizer, with at least one oscillator (VCO), filter (VCF), amplifier (VCA), envelope-generator (EG) or attack/decay/sustain/release (ADSR), and low-frequency oscillator (LFO), while also allowing for some interesting modulation between these different "modules" using a "modulation matrix". All the parameters of the different modules are user-controllable via the encoders, their respective buttons, and the touch-screen[^1].

Philip Schremp (a fellow EE) was my partner for this project and I had a blast working on this with him! We worked together on the entire concept from the ground up and divided up the different code modules between us. We both developed the system decomposition and project plan, researched and ordered parts from Digikey, and integrated the code modules together. Philip focused on the various signal processing components including the oscillators/LFOs, filters, ADSR, and DAC, whereas I focused on the external interfaces like the MIDI input circuitry, I2C encoder/button/LED interfaces, and their associated code modules and integrations. After the various code modules were done, we worked together to integrate everything and test/validate the design. Philip was also in charge of getting an enclosure 3D printed and getting a basic display* going, and I was in charge of connecting the different components together inside the enclosure.[^2]  

{% include image-gallery.html images="DAS_off.jpg" height="400" %}
  
The image below shows the top-level block diagram for the Digital Audio Synthesizer. The inputs to the unit are power (via a 5V USB connection, converting from 120 VAC), MIDI information (a 31.25kbps serial data-transfer protocol), and any user interactions with the device. The outputs from the unit are an audio signal (via a 1/4-inch TRS audio jack) and visual feedback for the user.  

{% include image-gallery.html images="top_level_diagram.png" height="400" %}
  
Starting from that top-level diagram, we performed a functional decomposition to get a more detailed diagram, shown below. The "Digital Signal Processor" and the "UI Engine" together make up the "Synth Engine", whose job is to take user input (and power) and spit out an audio signal and a form of visual user feedback. The Synth Engine is shown as two separate modules to visualize the fact that the **UI components** are almost entirely separate from the **synthesis components**. The synth part of the project operates on parameter values received from the UI part, and the UI part is constantly updating these parameters as the user desires.  

{% include image-gallery.html images="func_decomp_3.png" height="400" %}
  
The Synth Engine is the most complex part of the project, and it breaks down further as shown in the image below. (We did the same decomposition for the other modules but they won't be included here. Check the full report linked at the end for details[^2]).  

{% include image-gallery.html images="synth_engine.png" height="400" %}
  
[^1]: The touchscreen was not fully implemented due to time constraints. The display works, but the "touch" feature is not implemented. Also, the UI leaves a lot to be desired, and this is one area for future development.  

[^2]: Please check the following link for the full documentation for this project:  
[https://digitalcommons.calpoly.edu/eesp/646/](https://digitalcommons.calpoly.edu/eesp/646/)
