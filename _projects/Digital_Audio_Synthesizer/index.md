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

**Overview**
&nbsp;

The device takes user-controlled parameters (adjusted using encoders/buttons) and musical-instrument digital interface (MIDI) signals and uses them to synthesize electronic sounds. The goal was to emulate the standard analog subtractive synthesizer, with at least one oscillator (VCO), filter (VCF), amplifier (VCA), envelope-generator (EG) or attack/decay/sustain/release (ADSR), and low-frequency oscillator (LFO), while also allowing for some interesting modulation between these different "modules" using a "modulation matrix". All the parameters of the different modules are user-controllable via the encoders, their respective buttons, and the touch-screen[^1].

Philip Schremp (a fellow EE) was my partner for this project and I had a blast working on this with him! We worked together on the entire concept from the ground up and divided up the different code modules between us. We both developed the system decomposition and project plan, researched and ordered parts from Digikey, and integrated the code modules together. Philip focused on the various signal processing components including the oscillators/LFOs, filters, ADSR, and DAC, whereas I focused on the external interfaces like the MIDI input circuitry, I2C encoder/button/LED interfaces, and their associated code modules and integrations. After the various code modules were done, we worked together to integrate everything and test/validate the design. Philip was also in charge of getting an enclosure 3D printed and getting a basic display[^1] going, and I was in charge of connecting the different components together inside the enclosure.[^2]  

{% include image-gallery.html images="DAS_off.jpg" height="400" %}
&nbsp;

**System Block Diagrams**
&nbsp;

The image below shows the top-level block diagram for the Digital Audio Synthesizer. The inputs to the unit are power (via a 5V USB connection, converting from 120 VAC), MIDI information (a 31.25kbps serial data-transfer protocol), and any user interactions with the device. The outputs from the unit are an audio signal (via a 1/4-inch TRS audio jack) and visual feedback for the user.  

{% include image-gallery.html images="top_level_diagram.png" height="400" %}
&nbsp;

Starting from that top-level diagram, we performed a functional decomposition to get a more detailed diagram, shown below. The "Digital Signal Processor" and the "UI Engine" together make up the "Synth Engine", whose job is to take user input (and power) and spit out an audio signal and a form of visual user feedback. The Synth Engine is shown as two separate modules to visualize the fact that the **UI components** are almost entirely separate from the **synthesis components**. The synth part of the project operates on parameter values received from the UI part, and the UI part is constantly updating these parameters as the user desires.  

{% include image-gallery.html images="func_decomp_3.png" height="400" %}
&nbsp;

The Synth Engine is arguably the most complex part of the project, and it breaks down further as shown in the image below. (We did the same decomposition for the other modules but they won't be included here. Check the full report linked at the end for details[^2]).  

{% include image-gallery.html images="synth_engine.png" height="400" %}
&nbsp;

**Implementation**
&nbsp;

We decided to base this project around the STM32F746NG-DISCO discovery board, which includes an on-board DAC and touchscreen display[^1]. The STM32F746NG also has a dedicated floating-point unit (FPU) which is useful for DSP applications.

Many of the functional blocks have obvious implementations. For example, the power comes from a USB connection, the display acts as the primary visual feedback for the user, and the audio output comes from the on-board DAC (WM8994). Both the on-board DAC and the touchscreen display have HAL support, allowing for easier integration with the rest of the project components. This meant the majority of the project effort would be focused on the following components:
* DSP (synthesizer "modules")
* UI design (visual user-feedback)
* Controls (user inputs)
* MIDI input (musical note information)

As I mentioned earlier, Philip handled the majority of the synthesizer "module" programming. Philip used wavetables (arrays of sample values) for the oscillators and biquad topologies for the filters, and I refer the reader to our final report[^2] for more details regarding the DSP considerations.

For the UI and controls, we decided to use rotary encoders with built-in LEDs in addition to the touchscreen display[^1]. We used a total of four I2C rotary encoder boards from Adafruit, which include on-board "Neopixel" (WS281x) RGB LEDs. We supplied our own encoders for these boards so that we could pick the number of detents per rotation and choose encoders that include built-in push-buttons. The encoder boards are part of Adafruit's **Seesaw** family of products, meaning that they have direct library support in Arduino and CircuitPython development environments. Unfortunately, these libraries don't translate to the STM32 environment very well due to fundamental differences in programming methodology and standards between Arduino and STM32[^3]. Long story short, I ended up writing my own basic Adafruit Seesaw library that interfaces with the HAL's I2C handler, and then writing a separate module in that library specifically for the encoder boards. My library handles the I2C intialization, the Adafruit Seesaw initialization, and all the encoder, button, and LED interfaces. You can check out this library here: null

Finally, for the MIDI input, we were considering trying USB-MIDI, but given the time-constraints for this project we decided against it. We ended up using one of the STM32F7's UARTs with the tried-and-true standard MIDI-input circuit published my the MIDI Manufacturers Association way back in 1985, shown in the image below. Since the synthesizer doesn't need any MIDI output or MIDI "thru" capability, we ignored those parts of the schematic. I acquired the opto-coupler, diode, MIDI connector, and other specified components and soldered everything together on a perfboard. Then, I wrote my own MIDI library that interacts with the HAL's UART interfaces. You can find my MIDI library here: null

{% include image-gallery.html images="midi_circuit.png" height="400" %}
&nbsp;

**Conclusion**
&nbsp;

Looking back, if I were to embark on another project like this one, I would spend more time designing the enclosure. For this project we were getting very close to the deadline and needed to "just get the thing in a box," resulting in a barely sufficient enclosure.

Also, I would've spent more time thoroughly understanding the DSP aspects of the project. Philip handled most of the DSP for the project and that meant I wasn't able to help troubleshoot when things weren't working. It was quite a struggle getting each of the synth modules to behave when connected together.

Overall, this project went well, I enjoyed working with Philip on it, and there's lots of room for improvement. The UI/UX isn't that great currently, and the device isn't very "playable" currently from a musical standpoint, but given the strict time-constraints I am pretty happy with it. I hope to work more on this as time permits to improve the design and make it more musically-useful and fun to play! Please read the full report[^2] for more information about this project.

**Footnotes**
&nbsp;

[^1]: The touchscreen was not fully implemented due to time constraints. The display works, but the "touch" feature is not implemented. Also, the UI leaves a lot to be desired, and this is one area for future development.  

[^2]: Please check the following link for the full documentation for this project: [https://digitalcommons.calpoly.edu/eesp/646/](https://digitalcommons.calpoly.edu/eesp/646/)

[^3]: Arduino libraries are meant to be user-friendly and easy to use, but this means they handle all the hardware interface "magic" behind the scenes. On the other hand, STM provides a hardware abstraction layer (HAL) interface (meant to make software modules transferable between STM32 devices) but leaves a lot of the setup up to the programmer. While this might make STM32 more flexible by giving programmers more direct access to the lower-level (LL) programming interfaces, this also means that one cannot simply "initialize I2C" with a single line as in Arduino.
