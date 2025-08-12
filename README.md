# HuzAmp 
An electric bass guitar amplifier head capable of pushing 20 watts through 8-ohm speakers.
<p float="left">
  <img src="https://github.com/user-attachments/assets/be3368cf-199f-4710-bf0f-667f3fbe92b9" width="500" />
  <img src= "https://github.com/user-attachments/assets/21441805-2548-4da6-9d0c-d8c29421890c"  width="500" /> 
</p>

## Design Features
- Three Band EQ
- Class D power amplifier for over 90% efficiency
- Pre-amp bypass for external amplifiers
- Two speaker drivers for superior frequency response
## Motivation
After building my bass guitar, I was dissatisfied with the experience of simulating an amp digitally and interacting with the sound through standard headphones, as it lacked the feel that makes bass playing enjoyable. Learning about operational amplifiers and filters in my Circuits II class allowed me to consider designing my amplifier, apply my theoretical knowledge, and create something unique. 
## Pre-amp
A buffered instrument signal is run through active amp stages, first for a gain of about 11X, and then through a three-band equalization stage. Pictured below are the changes to the simulated signal voltage after each stage.

<img width="1913" height="854" alt="Image" src="https://github.com/user-attachments/assets/a1a5a7bc-988e-4cd0-ad20-54e0c082e74e" />
Notably, the equalization stage inverts and slightly reduces the amplitude of the wave. This is not an issue, as the audio application is not affected by a change in phase, and the loss of gain still allows us to fall inside the input voltage peak-to-peak requirement of the power amplifier.  


The three-band equalizer follows a Bandaxall design to shape the bass, mids, and treble. Due to the nature of Banaxall equilizers, manipulating a certain frequency band alters the shape of the other bands, leading to a more natural sound. Below is the Bode plot of the frequency range of the electric guitar when the potentiometer controlling the bass is at its maximum position, while the other two parameters are at their minimum.

<img width="1912" height="856" alt="Image" src="https://github.com/user-attachments/assets/cd48cdc3-2906-4c60-abf2-92cdb265bc73" />

## Power-amp
The power amp section is controlled by the TPA3116D2DADR class D amplifier. To create an amp that pushes around 20 watts, suitable for solo playing and light band use, I decided to use an input of 12V on an 8-ohm load.


