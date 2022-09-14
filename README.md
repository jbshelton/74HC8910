# 74HC8910
A clone of the AY-3-8910 sound chip made from 7400-series components. This will be an ongoing project, so what's included in this repository will reflect current progress with the project.

---

## Resources
I used [this repository](https://github.com/lvd2/ay-3-8910_reverse_engineered/) to help make the circuit.
I used [Digital](https://github.com/hneemann/Digital) (the logic simulator program) to initially build and test most of the logic.
I referred to ay-3-8910-core.vhd from [AS-2518-51_snd](https://github.com/FPGA-Code/AS-2518-51_snd) for the DAC output logic.

---

## NMOS to 7400- how to
The AY-3-8910 was a chip that used NMOS technology, short for N-channel Metal Oxide Semiconductor. NMOS only uses N-channel MOSFETs as well as pull-up resistors to perform logic functions.
Modern chips use CMOS, or Complimentary Metal Oxide Semiconductor, which uses both N-channel and P-channel MOSFETs to perform logic functions, which results in it being more power efficient and faster than NMOS. NMOS and CMOS gate construction and schematic representation is pretty different, so I'll go over how to decipher the schematics.

The reverse-engineered schematics don't represent a pull-up resistor as a resistor, but rather an N-channel MOSFET with its gate and source tied together. I won't go into detail of how it exactly works, but you can read the [Wikipedia](https://en.wikipedia.org/wiki/Depletion-load_NMOS_logic) article on it.

I still need to work on explaining how to identify the "basic" NMOS logic cells so this section is TODO

---

## The Journey
This is the start of my journey to recreating the AY-3-8910 using only 7400-series components. I will continually update my progress below after completing a stage of the circuit. Some parts may seem out of order to you, but to me, the order in which I re-engineered the circuit didn't matter, as I would be putting it together in chunks anyway.

---

### Step 1: the DAC decoder
I did the DAC decoder first because I wanted to get the digital/analog conversion out of the way. I wanted to use an 8-bit R2R DAC as the output, because using a 4-to-16 demultiplexer and 16 separate resistor dividers did not seem efficient to me, and the 8-bit DAC gave enough precision for the application. Initially, I made my own volume table by using an online decibel to equivalent linear value converter, and converted that into a circuit, but I then compared it against the values in the AY-3-8910 datasheet that were legible, and it didn't quite match. Fortunately, I found the AS-2518-51_snd repository, which had the 8-bit output truth table for the volume controller I was looking for. I changed the truth table values a bit to reduce the complexity- specifically the values 128, 64, 32, 16, 8, 4, 2 and 1 to 127, 63, 31, 15, 7, 3, 1 and 0 because those were just 255 (max value) shifted right, as the other half of the values were just 181 shifted right.

---

### Step 2: the register address decoder
The register address decoder is relatively simple- it uses 5-input NOR gates for each address, and either buffers the output or inverts the output for an active high or active low register select. I have not finished it yet so this section is TODO
