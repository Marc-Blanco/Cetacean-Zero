# 🐋 Cetacean Zero

A compact, open-source multi-tool handheld device inspired by the Flipper Zero, built around the **STMicroelectronics NUCLEO-F411RE** development board. It packs wireless communication, GNSS, NFC, IR transceiving, and a game-controller-style input layout into a single portable PCB.

---

## Overview
<img width="774" height="732" alt="image" src="https://github.com/user-attachments/assets/a336569c-fd51-47f4-843d-178d080de288" />
<img width="1176" height="776" alt="image" src="https://github.com/user-attachments/assets/a08ea83c-30e1-4a7b-ac20-871da27b608e" />


| Capability | Hardware | Interface |
|---|---|---|
| Sub-GHz RF transmit | Si4012-C1001xT | I²C (SPI-like via SCL/SDA) |
| NFC (contactless) | PN5321A3HN | I²C/SPI |
| GNSS positioning | u-blox MAX-M10S | UART |
| IR transmit/receive | Si4012 TX + TSOP38238 RX | GPIO / Timer PWM |
| Display | 2.8″ HS280S010B TFT (240×320) | SPI |
| Storage | MicroSD card slot | SPI |
| Power | USB-C input + TP4056 LiPo charger | — |
| Input | 8-button game controller layout | GPIO |
| Audio | Passive speaker/buzzer | GPIO (driver TBD) |

---

## Hardware Architecture

### Compute — NUCLEO64-F411RE
The STM32F411RE sits at the heart of the system. It runs at up to 100 MHz with an FPU (Cortex-M4), has 512 KB Flash and 128 KB RAM, and connects to all peripherals via its two 2×32 morpho connectors. The Nucleo's onboard ST-Link provides USB programming and UART debug out of the box.

### Power
USB-C (5 V) charges a single-cell LiPo battery via a **TP4056** linear charger IC. The CC1/CC2 pull-down resistors (5.1 kΩ each) are present for correct USB-C sink detection. Four 22 Ω series resistors cover the USB D+/D− pads for signal integrity.

> ⚠ **A 3.3 V LDO regulator and LiPo protection circuit are required** but not yet placed in the schematic. See the schematic review for details.

### Wireless
- **Sub-GHz RF (Si4012):** Transmits in the sub-GHz ISM bands. Frequency is set by an external 30 MHz crystal reference (not yet placed). Useful for replaying garage doors, remote controls, and other OOK/ASK signals.
- **NFC (PN5321A3HN):** Supports ISO 14443 / ISO 18092 for reading and emulating NFC-A/B/F cards and tags. Requires an external PCB coil antenna and matching circuit (not yet placed).
- **IR (TSOP38238 + Si4012):** 38 kHz IR receiver decodes remote control signals. IR transmission is handled via the Si4012's output stage driving an IR LED.

### GNSS
The **u-blox MAX-M10S** provides GPS/GNSS positioning. It communicates over UART to the STM32.

### Display & Storage
A 2.8″ 240×320 TFT (**HS280S010B**) connects over SPI for the main UI. A MicroSD card slot shares the SPI bus for firmware assets, signal capture logs, and config storage.

### Input
Eight tactile push buttons provide a Flipper-style navigation layout: D-pad (Up/Down/Left/Right), Back, and two shoulder buttons. A DPDT slide switch handles power on/off.

---

## Bill of Materials

See [`cetacean_zero_bom.csv`](./cetacean_zero_bom.csv) for the full component list with recommended footprints, LCSC part numbers where available, and per-component notes on outstanding issues.
