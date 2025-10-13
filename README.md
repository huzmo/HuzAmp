# HuzAmp 
An electric bass guitar amplifier head capable of pushing 20 watts through 8-ohm speakers.
<p float="left">
  <img src="https://github.com/user-attachments/assets/be3368cf-199f-4710-bf0f-667f3fbe92b9" width="450" />
  <img src= "https://github.com/user-attachments/assets/21441805-2548-4da6-9d0c-d8c29421890c"  width="450" /> 
</p>

## Demo
[Watch on YouTube](https://youtu.be/mhflcKGxFuk)

## Design Features
- Three Band EQ
- Class D power amplifier for over 90% efficiency
- Pre-amp bypass for external amplifiers
- Two speaker drivers for superior frequency response
## Motivation
After building my bass guitar, I was dissatisfied with the experience of simulating an amp digitally and interacting with the sound through standard headphones, as it lacked the feel that makes bass playing enjoyable. Learning about operational amplifiers and filters in my Circuits II class allowed me to consider designing my amplifier, apply my theoretical knowledge, and create something unique. 

## System Overview
<img width="1320" height="559" alt="Image" src="https://github.com/user-attachments/assets/432124b3-438c-4556-9b41-ff4e68bf42ff" />

A high-level overview of the signal path in this design.

## Pre-amp
A buffered instrument signal is run through active amp stages, first for a gain of about 11X, and then through a three-band equalization stage.

<img width="1913" height="854" alt="Image" src="https://github.com/user-attachments/assets/a1a5a7bc-988e-4cd0-ad20-54e0c082e74e" />
Notably, the equalization stage inverts and slightly reduces the amplitude of the wave. This is not an issue, as the audio application is not affected by a change in phase, and the loss of gain still allows us to fall inside the input voltage peak-to-peak requirement of the power amplifier.  


The three-band equalizer follows a Bandaxall design to shape the bass, mids, and treble. Due to the nature of Bandaxall equilizers, manipulating a certain frequency band alters the shape of the other bands, leading to a more natural sound. Below is the Bode plot of the frequency range of the electric guitar when the potentiometer controlling the bass is at its maximum position, while the other two parameters are at their minimum.

<img width="1912" height="856" alt="Image" src="https://github.com/user-attachments/assets/cd48cdc3-2906-4c60-abf2-92cdb265bc73" />

## Component Selection
### Op-Amps
The Texas Instruments OPA1641 was the op-amp of choice for the preamp and EQ stages due to its:
- **Low total harmonic distortion (THD)** of 0.00005%, ensuring transparency and minimal coloration of the bass tone  
- **Low input noise** (5.1 nV/√Hz), which helps maintain signal integrity from passive bass pickups  
- **Rail-to-rail output capability** and wide supply range (±2.25V to ±18V), offering headroom for dynamic signals  
- **Unity-gain stability**, making it ideal for both buffering and gain stages in the preamp chain
  
### Power Amp
The Texas Instruments TPA3116D2DADR Class-D amplifier module drives the speaker load efficiently, offering:
- **>90% efficiency**, minimizing heat and power waste  
- **0.1% THD at 1W** and high dynamic range  
- **Integrated protection features** including short-circuit, thermal, and undervoltage protection

These core components were selected with audio fidelity in mind while keeping conscious of form factor, cost, and thermals. 

## Hardware Design

### Layout
Component layout and placement were crucial elements in designing a quality product that is capable of producing a clean sound. I decided to use a 2-layer board in this design to minimize cost and complexity, but that meant that every decision was crucial in ensuring the system stays as noise-free as possible. By keeping analog circuitry physically separated from the power supply section, I was able to protect my sensitive signals from the noise of the switching regulators. 

### Routing and Signal Integrity
The analog signals, being sensitive, must be kept as isolated from noise and interference as possible. To prevent noise from the power supply section of the board, I employed the star ground method, which effectively isolates the analog and power grounds by connecting them at a single point using a ferrite bead. 

By routing horizontally on the top plane and vertically on the bottom plane, I minimized capacitive crosstalk by minimizing the coupled area between traces on adjacent layers. 

Alongside keeping short return paths, the components are laid out to keep similar signals together, minimizing interference. Via stitching is used liberally to keep return paths short and provide thermal relief between top and bottom ground pours, while also acting as a shield against electromagnetic interference.

### Thermal Considerations
The TPA3116D2DADR power amp IC sits on a dedicated copper pad with multiple thermal vias to the bottom layer, acting as a heat sink. Alongside the ground planes, the IC is thermally coupled to a 17 x 17 x 11.50mm aluminum heatsink with a thermal resistance of 23.91 °C/W (natural convection), providing adequate dissipation for a ~2 W thermal output at 20 W load, which provides additional heat dissipation to provide relief to the board. Due to the high efficiency of the Class-D topology, the heatsink operates at near room temperature even under a full 20W load, ensuring stable performance and long-term reliability without the need for active cooling. This passive design approach keeps the amplifier compact and silent while maintaining thermal headroom for extended playing sessions.

## Reflection
### Future Improvements
In terms of design, I would add a switch to power the power amp IC down if the user would only like to use the pre-amp. Another addition would be adding a lowpass filter with a corner frequency slightly above the maximum for our target bandwidth, which would ensure that any unwanted frequencies are handled before entering any tone-shaping stages.

### Closing Remarks
Through this project, I furthered my understanding of analog PCB design, power delivery networks, and signal integrity. From selecting the components to routing the traces, I was reminded of the importance of even the finest detail and how a small oversight could potentially alter the system's performance. The success of the first prototype validated the design workflow and the techniques employed to maintain system integrity. This project merged my interests in guitars and hardware design, allowing me to apply theory from my studies to a practical and personally meaningful product. 

