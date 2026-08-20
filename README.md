# CalcText

## What is CalcText?

- One or two sentences:
  - what it does
  - what physical form it uses
  - why you wanted to build it

![CalcText Rev A mainboard 3D render](docs/images/mainboard-3d.png)

## Design goals
- Preserve calculator form factor
- Battery powered
- LoRa communication
- Reuse original keypad
- Fit everything inside the existing enclosure

## Hardware overview
Briefly introduce the major parts:
- Wio-E5-LE
- TPS61299
- PCAL9535A
- HLCD290TB
- custom mainboard
- LCD interposer

## Mainboard
Explain:
- replaces original calculator PCB
- doubles as keypad contact board
- power circuitry
- MCU/radio
- GPIO expander
- LCD connection
- antenna connection

![Rev A mainboard PCB layout](docs/images/mainboard-layout.png)

## Power
Explain:
- single AAA
- approximate battery voltage range
- boost to 3.3 V
- why that architecture was chosen

## Keypad
Explain:
- original rubber keypad/carbon pills
- your custom exposed contacts
- 7×7 matrix
- GPIO expander
- separate ON/wake key
- any Rev A limitations such as ghosting

![Original calculator keypad and PCB](docs/images/original-keypad-assembly.jpg)

## Display
Explain:
- HLCD290TB
- resolution/type
- why you chose it
- physical constraint caused by its FPC

## LCD interposer
Explain:
- what it electrically does
- 24-pin display connection → 10-pin mainboard connection
- electrical design is complete
- final board outline/connector placement waits for the real LCD
- reason: physical FPC landing position/bend

![LCD interposer 3D render](docs/images/lcd-interposer-3d.png)

## RF / antenna
Explain:
- 868 MHz LoRa
- wire quarter-wave antenna
- approximate length
- where you intend to place it

## Mechanical integration
Explain:
- fitting everything inside the calculator
- acrylic/mainboard fit test
- display thickness / enclosure modifications
- PORON retention pads
- anything whose exact dimensions wait for physical assembly

![Acrylic mainboard fit test inside calculator enclosure](docs/images/mechanical-fit-test.jpg)

## Current status
- mainboard schematic complete
- mainboard PCB complete
- DRC clean
- interposer electrically complete
- procurement/BOM complete
- awaiting funding/build

## Bill of materials
- `bom.csv`
- Covers two CalcText units
- Don't duplicate the entire BOM in the README

## Next steps
1. Order parts/mainboards/displays
2. Assemble and bring up mainboard
3. Fit LCD physically
4. Finalise interposer geometry
5. Order interposer/FFC
6. Firmware
7. Full assembly/testing