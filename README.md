# CalcText

CalcText is a battery-powered LoRa communicator built into the shell and keypad of a standard CASIO fx-83GT X calculator. It is designed to preserve the calculator's original external form factor while integrating a custom radio, display, keypad interface and power system, all running from a single AAA battery.

This project is being developed for [Hack Club's Macondo](https://macondo.hackclub.com/projects/16515).

![CalcText Rev A mainboard 3D render](docs/images/mainboard-3d.png)

## Overview

The project started from a simple question: is it possible to message a friend using a calculator instead of a phone?

The calculator form factor is primarily an engineering constraint. CalcText explores low-power embedded design, RF, custom PCB design and mechanical integration within the very limited space of an existing calculator enclosure, while reusing its original keypad and external form.

## Hardware

### MCU and LoRa modem

The main controller is a Wio-E5-LE module, containing an STM32WLE5JC microcontroller and LoRa radio.

### Power architecture

- Single AAA battery, with its voltage expected to span roughly 0.8–1.5 V over its discharge
- TPS61299 boost converter regulating the main power rail to 3.3 V

### Display

- Hicenda HLCD290TB 2.9" reflective monochrome LCD
- 168 × 384 resolution
- Selected because it was the closest suitable display I could find to the dimensions of the original calculator screen

### Radio

- 868 MHz solid-core quarter-wave wire monopole antenna
- Intended for LoRa operation in the European 868 MHz ISM band

## Mainboard

The Rev A mainboard replaces the calculator's original PCB.

### Features

- Functions as the keypad contact PCB on its bottom layer
- Provides connections for the battery leads and wire antenna
- Contains the Wio-E5-LE, TPS61299 boost converter and PCAL9535A GPIO expander
- Contains the RF matching network between the Wio-E5-LE and antenna
- Provides the 10-pin connection to the LCD interposer

### Limitations

The HLCD290TB's integrated FPC cannot practically connect directly to the mainboard within the calculator enclosure, so a separate LCD interposer is required.

![Rev A mainboard PCB layout](docs/images/mainboard-layout.png)

## Display / LCD Interposer

The display uses an integrated 24-pin FPC. Its position and short length make it difficult to route directly to the mainboard inside the calculator.

The interposer therefore sits between the display and mainboard:

- The display's 24-pin FPC connects to a 24-pin connector on the interposer
- Display-specific power connections and decoupling are handled on the interposer
- The signals and power required from the mainboard are reduced to a 10-pin interface
- A short 10-way, 0.5 mm pitch FFC connects the interposer to the mainboard

The electrical design of the interposer is complete, but its final physical geometry is intentionally deferred until I have the real display.

The display manufacturer does not provide a minimum bend radius for its integrated FPC, so I do not want to guess its exact folded position. Once the display arrives, I can fit it naturally inside the enclosure and design the final interposer shape around where the FPC actually lands.

The interposer can then position its 10-pin connector so that the FFC to the mainboard runs approximately straight rather than having to bend sideways in its own plane.

![LCD interposer 3D render](docs/images/lcd-interposer-3d.png)

## Keypad

The original calculator uses an elastomeric keypad with conductive carbon pills which bridge contacts on its PCB when a key is pressed.

I decided to reuse the original keypad but replace the calculator PCB contacts with my own custom interdigitated exposed copper contacts on the bottom layer of the CalcText mainboard.

There are 49 matrix keys, arranged as a 7 × 7 matrix. Since the Wio-E5-LE does not have enough convenient GPIOs to directly handle the entire matrix alongside the rest of the hardware, the keypad interfaces with the MCU through a PCAL9535A GPIO expander.

The matrix does not currently use per-key diodes, so some combinations of simultaneous key presses could cause ghosting. This is acceptable for Rev A, but could be addressed in a later revision if it becomes a practical problem.

The calculator's ON key is handled separately from the 7 × 7 matrix and is also used as a wake input.

![Original calculator keypad and PCB](docs/images/original-keypad-assembly.jpg)

*Original fx-83GT X keypad assembly. The elastomer sheet uses conductive carbon pills which bridge contacts on the calculator PCB; CalcText reuses this keypad against custom interdigitated contacts on the Rev A mainboard.*

## Mechanical Integration

The original calculator enclosure is one of the main constraints of the project. The aim is to integrate the new electronics while preserving the calculator's original external form and making use of the limited internal space available.

Several internal modifications are still required to accommodate the new hardware, including expanding the available LCD area for the larger replacement display.

The antenna is intended to run vertically through otherwise unused space along the side of the enclosure, kept as far from other conductive material as reasonably possible.

The original AAA battery holder is retained, with the new mainboard positioned to use the existing battery compartment and approximately the same lead locations.

The replacement LCD is considerably thinner than the original calculator display and backplate assembly. I plan to use four small, very soft PORON pads behind it to lightly preload and retain the LCD against the front of the enclosure. The enclosure itself will provide the lateral positioning rather than relying on friction from the foam.

Final pad thickness and placement will be decided after physically fitting the display.

![Acrylic mainboard fit test inside calculator enclosure](docs/images/mechanical-fit-test.jpg)

*Acrylic fit test used to verify the Rev A mainboard outline, mounting-hole positions and available internal space against the original calculator enclosure.*

## Repository Structure

- `hardware/mainboard/` — KiCad project files for the Rev A mainboard
- `hardware/lcd-interposer/` — KiCad project files for the LCD interposer
- `hardware/libraries/` — custom footprints shared between the two boards
- `hardware/3d/` — additional component 3D models not supplied by KiCad
- `bom.csv` — complete bill of materials and procurement information for two CalcText devices

## Current Status

- Rev A mainboard schematic and PCB complete
- LCD interposer electrically complete
- Mainboard DRC clean
- Interposer checked, with final mechanical geometry intentionally deferred until physical display fit
- Procurement BOM prepared
- Awaiting funding and physical build

## Next Steps

1. Order the initial components, mainboards and displays
2. Assemble and bring up the Rev A mainboard
3. Physically fit the HLCD290TB into the calculator enclosure
4. Finalise the LCD interposer geometry and FFC length
5. Order the final interposer boards and mechanical retention material
6. Develop and test the firmware
7. Assemble and test the complete CalcText devices
