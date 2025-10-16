# HuzAmp 
An electric bass guitar amplifier head designed to push 20 watts through 4-ohm speakers.
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
After building my bass guitar, I was dissatisfied with the experience of simulating an amp digitally and interacting with the sound through standard headphones, as it lacked the feel that makes bass playing enjoyable. Learning about operational amplifiers and filters in my Circuits II class allowed me to consider designing my own amplifier by applying my theoretical knowledge and creating something unique. 

## System Overview
<img width="1320" height="559" alt="Image" src="https://github.com/user-attachments/assets/432124b3-438c-4556-9b41-ff4e68bf42ff" />

A high-level overview of the signal path in this design.

### Pre-Amp
A buffered instrument signal is run through active amp stages, first for a gain of about 11X, and then through a three-band equalization stage.

<img width="1913" height="854" alt="Image" src="https://github.com/user-attachments/assets/a1a5a7bc-988e-4cd0-ad20-54e0c082e74e" />
Notably, the equalization stage inverts and slightly reduces the amplitude of the wave. This is not an issue, as the audio application is not affected by a change in phase, and the loss of gain still allows us to fall inside the input voltage peak-to-peak requirement of the power amplifier.  


The three-band equalizer follows a Bandaxall design to shape the bass, mids, and treble. Due to the nature of Bandaxall equilizers, manipulating a certain frequency band alters the shape of the other bands, leading to a more natural sound. Below is the Bode plot of the frequency range of the electric guitar when the potentiometer controlling the bass is at its maximum position, while the other two parameters are at their minimum.

<img width="1912" height="856" alt="Image" src="https://github.com/user-attachments/assets/cd48cdc3-2906-4c60-abf2-92cdb265bc73" />

### Power Amp
<img width="1722" height="760" alt="Image" src="https://github.com/user-attachments/assets/6106d3f6-b894-403e-bce3-62d3dcba6744" />
The power amp circuit pays close attention to signal integrity, power stability, and EMI suppression. The chip is configured to receive a single-ended mono input and output a differential output. From the manufacturer's datasheet, the supplied power is capable of producing 20 W on a 4-ohm load.

<img width="334" height="217" alt="Image" src="https://github.com/user-attachments/assets/5407c7dd-0807-4725-9c75-e5d954e5d3a6" />

Keeping power delivery stable is imperative in this application. Decoupling capacitors are used where appropriate, through a combination of smaller capacitors to suppress higher-frequency noise and bulk decoupling to prevent larger voltage variations. Capacitors are placed in proximity to the VCC pins to minimize inductance in the power delivery path.

The output of the power amp is passed through an LC low-pass filter and an EMI snubber network. The role of the LC filter stage is to allow the band of usable audio only, and suppress any unwanted high-frequency noise from being outputted by the load. The EMI snubber stage is used to dissipate energy produced by high-frequency switching and stabilize the output filter when driving the reactive load. These stages are crucial in minimizing noise that could potentially ruin the listening experience from being outputted by the speaker.

### Power Delivery Network
<img width="1573" height="747" alt="Image" src="https://github.com/user-attachments/assets/fee8fa47-4ec4-431e-9dd2-83af3e07b2ef" />

12V is supplied to the device from an external DC power supply to ensure stable delivery without the need for a built-in supply using mains voltage. The 12V supply is fused at 3A to protect the circuit from a potentially damaging current surge. To provide a stable 5V for the analog supply, a step-down buck converter is employed, smoothed with an LC output network. The stable 5V is then passed into an isolated +/- 5V converter to provide a dual supply to the op-amps.

## Component Selection
### Op-Amps
The Texas Instruments OPA1641 was the op-amp of choice for the preamp and EQ stages due to its:
- Low total harmonic distortion (THD) of 0.00005%, ensuring transparency and minimal coloration of the bass tone  
- Low input noise (5.1 nV/√Hz), which helps maintain signal integrity from passive bass pickups  
- Rail-to-rail output capability and wide supply range (±2.25V to ±18V), offering headroom for dynamic signals  
- Unity-gain stability, making it ideal for both buffering and gain stages in the preamp chain
  
### Power Amp
The Texas Instruments TPA3116D2DADR Class-D amplifier module drives the speaker load efficiently, offering:
- >90% efficiency, minimizing heat and power waste  
- Simple mono configuration
- Integrated self-protection circuits, including short-circuit, thermal, and undervoltage protection

These core components were selected with audio fidelity in mind while keeping conscious of form factor, cost, and thermals. 

## Hardware Design

### Layout
Component layout and placement were crucial elements in designing a quality product that is capable of producing a clean sound. I decided to use a 2-layer board in this design to minimize cost and complexity, but that meant that every decision was crucial in ensuring the system stays as noise-free as possible. By keeping analog circuitry physically separated from the power supply section, I was able to protect my sensitive signals from the noise of the switching regulators. 

### Routing and Signal Integrity
The analog signals, being sensitive, must be kept as isolated from noise and interference as possible. To prevent noise from the power supply section of the board, I employed the star ground method, which effectively isolates the analog and power grounds by connecting them at a single point using a ferrite bead. 

By routing horizontally on the top plane and vertically on the bottom plane, I minimized capacitive crosstalk by minimizing the coupled area between traces on adjacent layers. 

Alongside keeping short return paths, the components are laid out to keep similar signals together, minimizing interference. Via stitching is used liberally to keep return paths short and provide thermal relief between top and bottom ground pours, while also acting as a shield against electromagnetic interference.

### Thermal Considerations
The TPA3116D2DADR power amp IC is soldered on a dedicated copper pad with multiple thermal vias to the bottom plane, acting as a heatsink to dissipate heat across the large ground plane. To support, the IC is paired with a 17 x 17 x 11.50mm aluminum heatsink, with a thermal resistance of 23.91 °C/W (natural convection) for heat dissipation of a ~2 W thermal output at 20 W load that provides additional relief to the board. Due to the extremely high efficiency of the Class-D topology, the heatsink is near room temperature even under a continuous 20W load, and reliable operation with long lifetime is obtained without the need for active cooling. This passive design approach keeps the amplifier compact and quiet with thermal headroom for extended playing times.

## Reflection
### Future Improvements
In terms of design, I would add a switch to power the power amp IC down if the user would only like to use the pre-amp. Another addition would be adding a lowpass filter with a corner frequency slightly above the maximum for our target bandwidth, which would ensure that any unwanted frequencies are handled before entering any tone-shaping stages.

### Closing Remarks
Through this project, I furthered my understanding of analog PCB design, power delivery networks, and signal integrity. From selecting the components to routing the traces, I was reminded of the importance of even the finest detail and how a small oversight could potentially alter the system's performance. The success of the first prototype validated the design workflow and the techniques employed to maintain system integrity. This project merged my interests in guitars and hardware design, allowing me to apply theory from my studies to a practical and personally meaningful product. 

