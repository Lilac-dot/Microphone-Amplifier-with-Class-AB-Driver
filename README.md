Microphone Amplifier with Class-AB Driver

A multi-stage transistor-based audio amplifier designed for amplifying weak microphone signals using a Class-AB push-pull output stage.

Overview

This project implements a microphone amplifier using BJTs (BC547 and BC557) along with passive components such as resistors, capacitors, and diodes. The amplifier consists of:

Input coupling stage
Voltage amplifier stage
Driver stage
Biasing network
Pre-driver stage
Class-AB push-pull output stage

The Class-AB configuration improves efficiency while reducing crossover distortion.

Features
Multi-stage transistor amplifier
Class-AB push-pull output
Reduced crossover distortion using diode biasing
Improved current driving capability
Suitable for microphone and small audio signal amplification
Components Used
BC547 (NPN transistor)
BC557 (PNP transistor)
1N4148 diodes
Capacitors (10µF, 100µF, 220µF)
Biasing resistors
12V power supply
Working Principle

The weak microphone signal is first coupled through a capacitor to remove DC components. A common-emitter transistor stage provides voltage amplification. The signal then passes through driver and pre-driver stages to improve current capability.

Two diodes provide approximately 1.4V biasing for the Class-AB push-pull output stage, reducing crossover distortion and improving signal quality.

Circuit Diagram

<img width="780" height="477" alt="image" src="https://github.com/user-attachments/assets/6363d1cc-5bc5-4756-bd23-3397afe23cd0" />

LTspice Simulation

The circuit was simulated using LTspice to verify amplification and output waveform characteristics.

Output Waveform

<img width="780" height="397" alt="image" src="https://github.com/user-attachments/assets/6b137fa5-f550-40c8-bd03-6df2d5b1df24" />

Applications
Microphone amplifiers
Audio amplification systems
Public address systems
Portable speakers
Communication equipment
Future Improvements
PCB implementation
Noise reduction techniques
Tone control integration
Higher power output stage
Battery-powered portable version
