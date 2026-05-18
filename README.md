<div align="center">

### Microphone Amplifier with Class-AB Driver

A discrete transistor-based audio amplifier that takes microphone-level signals and delivers amplified output through a low-distortion Class-AB push-pull stage, designed and simulated from first principles.

</div>

---

## Project Overview

Microphones and other audio transducers produce signals in the millivolt range — far too weak to drive any meaningful load directly. Audio amplification is required to raise both the voltage and current of these signals to levels capable of operating speakers, headphone drivers, or downstream processing circuits.

This project presents a multi-stage, transistor-based amplifier built entirely from discrete BJT components. The circuit progresses through carefully designed stages: signal coupling, voltage amplification, current buffering, and finally a Class-AB push-pull output stage that delivers the amplified signal to a load.

Class-AB operation is specifically chosen over simpler configurations for two reasons:

- Class-A amplifiers are linear but wasteful — the transistor conducts continuously, dissipating significant power even with no signal.
- Class-B amplifiers are efficient but introduce **crossover distortion** — a dead zone near the zero-crossing where neither transistor conducts.
- Class-AB biases both output transistors into slight conduction at all times using a diode network, eliminating the crossover dead zone while retaining reasonable efficiency.

The result is a clean, amplified audio signal with reduced distortion, suitable for practical audio applications.

---

## Features

- Multi-stage signal amplification using discrete NPN and PNP BJTs
- Class-AB push-pull output stage for high efficiency and low distortion
- Diode biasing network providing ~1.4 V quiescent bias to output transistors
- AC-coupled input and output — complete DC isolation from source and load
- Voltage gain from common-emitter stage; current gain from emitter-follower stages
- Capable of driving a resistive load or speaker from a microphone-level input
- Designed and verified using LTspice transient simulation

---

## Components Used

| Component | Value / Part No. | Purpose |
|---|---|---|
| NPN Transistor | BC547B (Q1, Q2, Q4) | Common-emitter amplifier, driver, pre-driver stages |
| PNP Transistor | BC557B (Q5) | Complementary output transistor in push-pull stage |
| Signal Diodes | 1N4148 (D1, D2) | Provide ~1.4 V bias to eliminate crossover distortion |
| Input Coupling Capacitor | C1 — 10 µF | Blocks DC, passes AC signal from source |
| Interstage Capacitors | C2 — 100 µF, C4 — 10 µF | Coupling between amplifier stages |
| Output Coupling Capacitor | C5 — 220 µF | Delivers amplified AC signal to the load |
| Bias / Bypass Capacitors | C3 — 220 µF | Bypasses emitter resistor for AC gain |
| Biasing Resistors | R1–R12 (various) | Set quiescent operating points across all stages |
| Load Resistor / Speaker | R11 — 1 Ω (load) | Output load representing speaker impedance |
| Power Supply | 12 V DC | Powers all transistor stages |

---

## Software and Tools

| Tool | Purpose |
|---|---|
| LTspice XVII | Schematic entry, SPICE simulation, waveform analysis |
| Embedded Circuit Analysis | Hand calculations for bias points and gain estimation |
| BJT Datasheet (BC547 / BC557) | Component characterization and operating region verification |

---

## Circuit Architecture

```
┌──────────────────────────────────┐
│    Input Signal (microphone)     │  ~1 mV–10 mV AC, 1 kHz
└──────────────┬───────────────────┘
               │
               ▼
┌──────────────────────────────────┐
│      Input Coupling Capacitor    │  C1 = 10 µF — blocks DC, passes AC
└──────────────┬───────────────────┘
               │
               ▼
┌──────────────────────────────────┐
│     Voltage Amplifier Stage      │  Q1 (BC547) — Common Emitter
│   Voltage Divider Bias Network   │  R1, R2, R3, R4 set operating point
└──────────────┬───────────────────┘
               │
               ▼
┌──────────────────────────────────┐
│         Driver Stage             │  Q2 (BC547) — Emitter Follower
│   Increases current capability   │  High input impedance, low output Z
└──────────────┬───────────────────┘
               │
               ▼
┌──────────────────────────────────┐
│    Diode Biasing Network         │  D1, D2 (1N4148) — ~1.4 V bias
│   Keeps output BJTs conducting   │  Eliminates crossover dead zone
└──────────────┬───────────────────┘
               │
               ▼
┌──────────────────────────────────┐
│       Pre-Driver Stage           │  Q4 (BC547) — additional current gain
└──────────────┬───────────────────┘
               │
               ▼
┌──────────────────────────────────┐
│   Class-AB Push-Pull Output      │  Q4 NPN + Q5 PNP (BC547 / BC557)
│  NPN conducts on positive half   │  PNP conducts on negative half
└──────────────┬───────────────────┘
               │
               ▼
┌──────────────────────────────────┐
│    Output Coupling Capacitor     │  C5 = 220 µF — passes AC to load
└──────────────┬───────────────────┘
               │
               ▼
┌──────────────────────────────────┐
│      Speaker / Load (R11)        │  Amplified audio output delivered
└──────────────────────────────────┘
```

---

## Working Principle

1. Input Coupling — The weak microphone signal enters through C1 (10 µF), which strips any DC offset and passes only the AC audio waveform to the first transistor stage.

2. Voltage Amplification — Q1 (BC547) is configured as a common-emitter amplifier. A voltage divider formed by R1–R4 biases Q1 in its active region. This stage delivers significant voltage gain, substantially increasing the amplitude of the signal.

3. Driver Stage — Q2 (BC547) operates as an emitter follower (common-collector). It does not add voltage gain but presents a high input impedance and low output impedance, allowing the stage to source the current required to drive subsequent stages without loading the voltage amplifier.

4. Diode Biasing — Two 1N4148 diodes (D1, D2) in series develop a forward voltage of approximately 1.4 V. This is applied between the bases of the output transistors, keeping them in slight conduction at all times. The result is Class-AB operation — the dead zone present in Class-B designs is eliminated.

5. Pre-Driver and Push-Pull Amplification — Q4 provides additional current drive. The complementary output pair (Q4 NPN / Q5 PNP) operates in push-pull: the NPN transistor sources current during the positive half-cycle and the PNP transistor sinks current during the negative half-cycle. Together they faithfully reproduce the full waveform at high current levels.

6. Output Delivery — C5 (220 µF) couples the amplified AC signal to the load while blocking the DC quiescent voltage, ensuring only the audio signal reaches the speaker.

---

## Amplifier Stages in Detail

### Input Stage
The input coupling capacitor acts as a high-pass filter, removing any DC present at the microphone output. This protects the transistor's bias point from being disturbed by source DC levels. Only the audio-frequency AC signal is admitted.

### Voltage Amplifier Stage
Q1 in common-emitter configuration is the primary gain stage. In this topology, the collector current varies in proportion to the base current, and since the collector load resistor is much larger than the emitter resistance, the voltage gain is substantial. The emitter bypass capacitor (C3) short-circuits the emitter resistor at AC frequencies, maximizing gain while preserving DC stability.

### Driver Stage
Q2 acts as a unity-voltage-gain buffer. Its purpose is impedance transformation — stepping down the output impedance of the voltage amplifier so that the next stage receives a signal from a low-impedance source, minimizing signal loss and improving drive capability.

### Biasing Network
The two 1N4148 diodes are thermally matched to the output transistors in practical designs. They establish a stable forward-bias voltage that tracks with temperature, maintaining consistent quiescent current through the output pair across operating conditions.

### Pre-Driver Stage
Q4 provides the final stage of current amplification before the output transistors. It ensures the base currents of the large output devices are fully supplied without drawing excessive current from the driver stage.

### Class-AB Push-Pull Output Stage
The complementary pair (NPN + PNP) operates symmetrically. Each transistor handles one half of the signal cycle. Because both are biased just above cutoff by the diode network, conduction is continuous through the crossover region — eliminating the distortion artifact that defines Class-B operation.

---

## Class-AB Operation

| Amplifier Class | Conduction Angle | Efficiency | Distortion |
|---|---|---|---|
| Class-A | 360° (full cycle) | Low (~25%) | Very Low |
| Class-B | 180° (half cycle each) | High (~78%) | High (crossover distortion) |
| Class-AB | Slightly > 180° each | Moderate–High | Low |

**Crossover distortion** occurs in Class-B designs because both transistors are biased at cutoff. Near the zero-crossing of the waveform, there is a brief interval where neither transistor conducts, creating a notch in the output that is audible as distortion.

**Diode biasing** resolves this: by holding both output transistors slightly on at all times, the transition from one device to the other is smooth. The output follows the input continuously through the zero crossing. The efficiency penalty over Class-B is small, while the distortion benefit is substantial — making Class-AB the standard choice in audio amplifier design.

---

## Design Notes

- **Coupling Capacitors** — C1, C2, C4, and C5 establish AC signal paths between stages. Their values are chosen so that the associated high-pass corner frequencies fall well below the audio band (typically below 20 Hz).
- **Voltage Divider Biasing** — Resistor pairs at each transistor base set the quiescent operating point. The divider current is designed to be significantly larger than the base current, making the bias stable and relatively insensitive to transistor beta variation.
- **Emitter Follower Behavior** — Driver and pre-driver stages in emitter-follower configuration exhibit near-unity voltage gain, high input impedance, and low output impedance — ideal properties for interstage buffering.
- **Complementary Symmetry** — The BC547 (NPN) and BC557 (PNP) are selected as a complementary pair with closely matched characteristics, ensuring the positive and negative half-cycles of the output are amplified symmetrically.

---

## Simulation Results

The circuit was simulated in LTspice using a 1 kHz sinusoidal input at 1 mV amplitude, with a 12 V supply.

| Parameter | Observed Result |
|---|---|
| Input Signal | ~1 mV peak, 1 kHz |
| Output Voltage Swing | ~500–600 mV peak |
| Voltage Gain | Approximately 500x across the full chain |
| Crossover Distortion | Not observed — diode biasing effective |
| Output Waveform | Clean sinusoidal output at load |
| Push-Pull Operation | Confirmed: NPN and PNP conduction alternating per half-cycle |

The LTspice waveform plots confirm successful signal amplification and proper Class-AB behavior with no visible crossover artifact.

### Simulation Waveforms

| View | File |
|---|---|
| Full Circuit Schematic | <img width="780" height="480" alt="image" src="https://github.com/user-attachments/assets/eeb7591d-9834-48ba-9083-0d2942bce7cd" /> |
| Input vs Output Overlay | <img width="781" height="402" alt="image" src="https://github.com/user-attachments/assets/b770a402-e401-4f1f-b380-a12554220b70" /> |


---

## Advantages

- Higher power efficiency compared to Class-A designs, with no continuous full-power dissipation
- Crossover distortion eliminated by diode biasing, unlike Class-B configurations
- Complementary push-pull topology allows symmetrical amplification of both signal half-cycles
- Simple discrete design — no integrated circuits required
- Suitable for battery-operated or low-supply-voltage audio applications

---

## Applications

- Microphone preamplifier and signal conditioning circuits
- Portable and desktop audio amplifier systems
- Public address and intercom systems
- Communication equipment front-ends
- Educational platforms for studying analog amplifier design

---

## Setup and Simulation Instructions

### Running the LTspice Simulation

**1. Install LTspice**

Download LTspice XVII (free) from [Analog Devices](https://www.analog.com/en/resources/design-tools-and-calculators/ltspice-simulator.html) and install on Windows or macOS.

**2. Open the Schematic**

```
File > Open > LTspice_Simulation/microphone_amplifier.asc
```

**3. Configure the Input Source**

The input voltage source V1 should be configured as:

```
SINE(0 1m 1K)
```

This sets a 1 mV amplitude, 1 kHz sinusoidal signal — representative of a microphone output.

**4. Run the Transient Simulation**

```
Simulate > Edit Simulation Command > Transient
Stop time: 10ms
Maximum timestep: 1us
```

Click **Run**.

**5. Observe Waveforms**

Click on the input node (V1) and output node (across R11) to plot both waveforms. Use the cursor tool to measure peak voltages and calculate voltage gain.

**6. Verify Class-AB Operation**

Plot the collector currents of Q4 (NPN) and Q5 (PNP) simultaneously. You should observe each transistor conducting on alternate half-cycles with a smooth transition at the zero crossing — confirming Class-AB push-pull operation.

---

## Conclusion

This project demonstrates a complete, multi-stage discrete transistor amplifier designed for microphone signal amplification. Starting from a millivolt-level input, the circuit progressively raises the signal through a common-emitter voltage amplifier, emitter-follower driver stages, and a diode-biased Class-AB push-pull output stage — delivering a clean, amplified waveform to the load.

The LTspice simulation confirms that diode biasing successfully eliminates crossover distortion, and that the complementary output pair operates symmetrically across both half-cycles. The design strikes a deliberate balance between the low-distortion properties of Class-A operation and the efficiency advantages of Class-B, making it well suited to practical audio amplification applications.

---

## References

> [1] A. S. Sedra and K. C. Smith, *Microelectronic Circuits*, 7th ed. Oxford University Press, 2014.

> [2] R. L. Boylestad and L. Nashelsky, *Electronic Devices and Circuit Theory*, 11th ed. Pearson Education, 2013.

> [3] BC547 / BC557 Datasheets — Fairchild Semiconductor / ON Semiconductor.

---

<div align="center">
