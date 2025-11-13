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

The device takes user-controlled parameters (adjusted using encoders/buttons) and musical-instrument digital interface (MIDI) signals and uses them to synthesize electronic sounds. The goal was to emulate the standard analog subtractive synthesizer, with at least one oscillator (VCO), filter (VCF), amplifier (VCA), envelope-generator (EG) or attack/decay/sustain/release (ADSR), and low-frequency oscillator (LFO), while also allowing for some interesting modulation between these different "modules" using a "modulation matrix". All the parameters of these different modules are controllable by the user using the encoders, their respective buttons, and (ideally) the touch-screen.

Philip Schremp (a fellow EE) was my partner for this project and I had a blast working on this with him! We worked together on the entire concept from the ground up and divided up the different code modules between us. We both developed the system decomposition and project plan, researched and ordered parts from Digikey, and integrated the code modules together. Philip focused on the various signal processing components including the oscillators/LFOs, filters, and ADSR, whereas I focused on the external interfaces like the MIDI input circuitry, I2C encoder/button/LED interfaces, and their associated code modules and integrations. After the various code modules were done, we worked together to integrate everything together (system integration).

Please check the following link for the full documentation for this project:
https://digitalcommons.calpoly.edu/eesp/646/
