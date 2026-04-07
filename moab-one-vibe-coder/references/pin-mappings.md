# MOAB-ONE Pin Mappings Reference

This document maps every MOAB-ONE standardized signal to the corresponding MCU pin for each of the five supported development board form factors.

## Standardized MOAB Signal Bus

MOAB-ONE routes MCU pins to a **standardized signal bus** so that peripherals see the same signals regardless of which MCU board is mounted. The key signal groups are:

| Signal Group | Signals | Purpose |
|-------------|---------|---------|
| I2C Bus 0 | SDA0, SCL0 | Primary I2C — most peripherals |
| I2C Bus 1 | SDA1/D8, SCL1/D9 | Secondary I2C |
| UART 0 | TX0, RX0 | Primary serial |
| UART 1 | TX1/D10, RX1/D11 | Secondary serial |
| SPI Bus 0 | MOSI0, MISO0, CLK0 | Primary SPI |
| SPI Bus 1 | MOSI1/D12, MISO1/D13, CLK1/D14 | Secondary SPI |
| Analog/Digital | A0/D0 through A7/D7 | GPIO + ADC |
| Digital Only | D15, D16, D17, D18 | Extra GPIO |
| Reference | AREF | ADC reference |
| Power | 3.3V, 5V, GND | Power rails |
| Control | RESET, EN | Board control |

---

## Adafruit Feather (28-pin, 16+12 headers)

Mount position: Align left bottom pin to the **Feather** label on the MDB socket.

| MOAB Pin | MOAB Signal | Feather Pin | nRF52840 GPIO | Arduino Constant |
|:--------:|-------------|-------------|---------------|-----------------|
| 1 | RESET | RESET | P0.18 | — |
| 2 | 3.3V | 3V3 | — | — |
| 3 | AREF | AREF | P0.04 | — |
| 4 | GND | GND | — | — |
| 5 | A0/D0 | A0 | P0.02 | `A0` |
| 6 | A1/D1 | A1 | P0.03 | `A1` |
| 7 | A2/D2 | A2 | P0.28 | `A2` |
| 8 | A3/D3 | A3 | P0.29 | `A3` |
| 9 | A4/D4 | A4 | P0.30 | `A4` |
| 10 | A5/D5 | A5 | P0.31 | `A5` |
| 11 | CLK0 | SCK | P1.13 | `SCK` |
| 12 | MOSI0 | MOSI | P1.15 | `MOSI` |
| 13 | MISO0 | MISO | P1.14 | `MISO` |
| 14 | RX0 | RX | P0.24 | `RX` / `0` |
| 15 | TX0 | TX | P0.25 | `TX` / `1` |
| 16 | SDA0 | SDA | P0.12 | `SDA` |
| 17 | SCL0 | SCL | P0.11 | `SCL` |
| 18 | A6/D6 | D5 | P0.27 | `5` |
| 19 | A7/D7 | D6 | P0.07 | `6` |
| 20 | D15 | D9 | P0.26 | `9` |
| 21 | D16 | D10 | P0.06 | `10` |
| 22 | D17 | D11 | P0.08 | `11` |
| 23 | D18 | D12 | P1.09 | `12` |
| 24 | D13 | D13 | P0.14 | `13` |
| 25 | EN | EN | — | — |
| 26 | VBUS | USB | — | — |
| 27 | — | NC | — | — |
| 28 | VBAT | BAT | — | — |

> **Note:** Feather boards from different manufacturers may have slight differences on AREF and some GPIO pins. Always verify against your specific board's pinout diagram.

---

## Arduino Nano (30-pin, 2×15 headers)

Mount position: Align left bottom pin to the **Nano** label on the MDB socket.

| MOAB Pin | MOAB Signal | Nano Pin | nRF52840 GPIO | Arduino Constant |
|:--------:|-------------|----------|---------------|-----------------|
| 1 | — | D1/CLKO | — | `1` |
| 2 | 3.3V | +3V3 | — | — |
| 3 | AREF | AREF | — | — |
| 4 | A0/D0 | A0 | P0.04 | `A0` / `14` |
| 5 | A1/D1 | A1 | P0.05 | `A1` / `15` |
| 6 | A2/D2 | A2 | P0.30 | `A2` / `16` |
| 7 | A3/D3 | A3 | P0.29 | `A3` / `17` |
| 8 | A4/D4 | A4/SDA | P0.31 | `A4` / `18` |
| 9 | A5/D5 | A5/SCL | P0.02 | `A5` / `19` |
| 10 | A6/D6 | A6 | P0.28 | `A6` / `20` |
| 11 | A7/D7 | A7 | P0.03 | `A7` / `21` |
| 12 | CLK0 | D13/SCK | P1.10 | `13` |
| 13 | MOSI0 | D11/MOSI | P1.13 | `11` |
| 14 | MISO0 | D12/MISO | P1.11 | `12` |
| 15 | RX0 | D0/RX | P1.10 | `0` |
| 16 | TX0 | D1/TX | P1.03 | `1` |
| 17 | GND | GND | — | — |
| 18 | 5V | +5V | — | — |
| 19 | RESET | RESET | P0.18 | — |
| 20 | GND | GND | — | — |
| 21 | SDA0 | A4/SDA | P0.31 | `SDA` |
| 22 | SCL0 | A5/SCL | P0.02 | `SCL` |
| 23 | TX1/D10 | D2 | P1.11 | `2` |
| 24 | RX1/D11 | D3 | P1.12 | `3` |
| 25 | SDA1/D8 | D4 | P1.15 | `4` |
| 26 | SCL1/D9 | D5 | P1.13 | `5` |
| 27 | MOSI1/D12 | D6 | P1.14 | `6` |
| 28 | MISO1/D13 | D7 | P0.23 | `7` |
| 29 | CLK1/D14 | D8 | P0.21 | `8` |
| 30 | D15 | D9 | P0.27 | `9` |

---

## Raspberry Pi Pico (40-pin, 2×20 headers)

Mount position: Align left bottom pin to the **Pico** label on the MDB socket.

| MOAB Pin | MOAB Signal | Pico Pin | RP2040/RP2350 GPIO | Arduino Constant |
|:--------:|-------------|----------|-------------------|-----------------|
| 1 | TX0 | Pin 1 | GP0 (UART0 TX) | `0` |
| 2 | RX0 | Pin 2 | GP1 (UART0 RX) | `1` |
| 3 | GND | Pin 3 | GND | — |
| 4 | SDA1/D8 | Pin 4 | GP2 (I2C1 SDA) | `2` |
| 5 | SCL1/D9 | Pin 5 | GP3 (I2C1 SCL) | `3` |
| 6 | SDA0 | Pin 6 | GP4 (I2C0 SDA) | `4` |
| 7 | SCL0 | Pin 7 | GP5 (I2C0 SCL) | `5` |
| 8 | GND | Pin 8 | GND | — |
| 9 | A3/D3 | Pin 9 | GP6 | `6` |
| 10 | A4/D4 | Pin 10 | GP7 | `7` |
| 11 | TX1/D10 | Pin 11 | GP8 (UART1 TX) | `8` |
| 12 | RX1/D11 | Pin 12 | GP9 (UART1 RX) | `9` |
| 13 | GND | Pin 13 | GND | — |
| 14 | A5/D5 | Pin 14 | GP10 | `10` |
| 15 | D15 | Pin 15 | GP11 | `11` |
| 16 | GND | Pin 16 | GND | — |
| 17 | MISO1/D13 | Pin 17 | GP12 (SPI1 RX) | `12` |
| 18 | A7/D7 | Pin 18 | GP13 | `13` |
| 19 | GND | Pin 19 | GND | — |
| 20 | MOSI1/D12 | Pin 20 | GP14 (SPI1 TX) | `14` |
| 21 | CLK1/D14 | Pin 21 | GP15 (SPI1 SCK) | `15` |
| 22 | MOSI0 | Pin 22 | GP16 (SPI0 TX) | `16` |
| 23 | CLK0 | Pin 23 | GP17 (SPI0 SCK) | `17` |
| 24 | GND | Pin 24 | GND | — |
| 25 | A6/D6 | Pin 25 | GP18 | `18` |
| 26 | MISO0 | Pin 26 | GP19 (SPI0 RX) | `19` |
| 27 | GND | Pin 27 | GND | — |
| 28 | D16 | Pin 28 | GP20 | `20` |
| 29 | D17 | Pin 29 | GP21 | `21` |
| 30 | D18 | Pin 30 | GP22 | `22` |
| 31 | GND | Pin 31 | GND | — |
| 32 | A0/D0 | Pin 32 | GP26 (ADC0) | `26` / `A0` |
| 33 | A1/D1 | Pin 33 | GP27 (ADC1) | `27` / `A1` |
| 34 | GND (AGND) | Pin 34 | AGND | — |
| 35 | A2/D2 | Pin 35 | GP28 (ADC2) | `28` / `A2` |
| 36 | AREF (ADC_VREF) | Pin 36 | ADC_VREF | — |
| 37 | 3.3V | Pin 37 | 3V3(OUT) | — |
| 38 | GND | Pin 38 | GND | — |
| 39 | 5V (VSYS) | Pin 39 | VSYS | — |
| 40 | VBUS | Pin 40 | VBUS | — |

> **Note on analog pins:** Pico has only 3 true ADC channels (GP26–GP28 = A0–A2). Signals labeled A3–A7 on the MOAB are digital-only GPIO on the Pico. Use external ADC if more analog channels are needed.

---

## Seeed Studio XIAO / Adafruit QT Py (14-pin, 2×7 headers)

MOAB-ONE has **two XIAO/QT Py slots**. Both can be populated simultaneously.

### XIAO Slot 0 (XIAO_0) — Bus 0 Signals

Mount position: Align to the **XIAO_0** / **QT Py** label.

| MOAB Pin | MOAB Signal | XIAO Pin | ESP32-S3 Example | Arduino Constant |
|:--------:|-------------|----------|-----------------|-----------------|
| 1 | A0/D0 | D0/A0 | GPIO1 | `A0` / `D0` |
| 2 | A1/D1 | D1/A1 | GPIO2 | `A1` / `D1` |
| 3 | A2/D2 | D2/A2 | GPIO3 | `A2` / `D2` |
| 4 | A3/D3 | D3/A3 | GPIO4 | `A3` / `D3` |
| 5 | SDA0 | D4/SDA | GPIO5 | `SDA` / `D4` |
| 6 | SCL0 | D5/SCL | GPIO6 | `SCL` / `D5` |
| 7 | TX0 | D6/TX | GPIO43 | `TX` / `D6` |
| 8 | 5V | 5V | — | — |
| 9 | GND | GND | — | — |
| 10 | 3.3V | 3V3 | — | — |
| 11 | MOSI0 | D10/MOSI | GPIO9 | `MOSI` / `D10` |
| 12 | MISO0 | D9/MISO | GPIO8 | `MISO` / `D9` |
| 13 | CLK0 | D8/SCK | GPIO7 | `SCK` / `D8` |
| 14 | RX0 | D7/RX | GPIO44 | `RX` / `D7` |

### XIAO Slot 1 (XIAO_1) — Bus 1 Signals

Mount position: Align to the **XIAO_1** label.

| MOAB Pin | MOAB Signal | XIAO Pin | ESP32-S3 Example | Arduino Constant |
|:--------:|-------------|----------|-----------------|-----------------|
| 1 | A1/D1 | D0/A0 | GPIO1 | `A0` / `D0` |
| 2 | A2/D2 | D1/A1 | GPIO2 | `A1` / `D1` |
| 3 | A3/D3 | D2/A2 | GPIO3 | `A2` / `D2` |
| 4 | A7/D7 | D3/A3 | GPIO4 | `A3` / `D3` |
| 5 | SDA1/D8 | D4/SDA | GPIO5 | `SDA` / `D4` |
| 6 | SCL1/D9 | D5/SCL | GPIO6 | `SCL` / `D5` |
| 7 | TX1/D10 | D6/TX | GPIO43 | `TX` / `D6` |
| 8 | 5V | 5V | — | — |
| 9 | GND | GND | — | — |
| 10 | 3.3V | 3V3 | — | — |
| 11 | MOSI1/D12 | D10/MOSI | GPIO9 | `MOSI` / `D10` |
| 12 | MISO1/D13 | D9/MISO | GPIO8 | `MISO` / `D9` |
| 13 | CLK1/D14 | D8/SCK | GPIO7 | `SCK` / `D8` |
| 14 | RX1/D11 | D7/RX | GPIO44 | `RX` / `D7` |

> **Critical:** XIAO_0 connects to bus 0 peripherals (I2C0, SPI0, UART0). XIAO_1 connects to bus 1 peripherals. If using a single XIAO, prefer slot 0 since most MOAB peripheral connectors default to bus 0 signals.

---

## Raspberry Pi Zero (40-pin, 2×20 headers)

Mount position: Align to the **Zero** label on the MDB socket.

| MOAB Pin | MOAB Signal | Zero Pin | BCM GPIO | Arduino/WiringPi |
|:--------:|-------------|----------|----------|-----------------|
| 1 | SDA0 | Pin 3 | GPIO 2 (SDA1) | `2` |
| 2 | 5V | Pin 2 | 5V | — |
| 3 | SCL0 | Pin 5 | GPIO 3 (SCL1) | `3` |
| 4 | 5V | Pin 4 | 5V | — |
| 5 | RESET | Pin 7 | GPIO 4 | `4` |
| 6 | GND | Pin 6 | GND | — |
| 7 | EN | Pin 9 | GPIO 17 | `17` |
| 8 | TX0 | Pin 8 | GPIO 14 (TXD) | `14` |
| 9 | A0/D0 | Pin 11 | GPIO 27 | `27` |
| 10 | RX0 | Pin 10 | GPIO 15 (RXD) | `15` |
| 11 | A1/D1 | Pin 13 | GPIO 22 | `22` |
| 12 | D15 | Pin 12 | GPIO 18 | `18` |
| 13 | MOSI0 | Pin 15 | GPIO 10 (MOSI) | `10` |
| 14 | GND | Pin 14 | GND | — |
| 15 | MISO0 | Pin 17 | GPIO 9 (MISO) | `9` |
| 16 | TX1/D10 | Pin 16 | GPIO 23 | `23` |
| 17 | CLK0 | Pin 19 | GPIO 11 (SCLK) | `11` |
| 18 | RX1/D11 | Pin 18 | GPIO 24 | `24` |
| 19 | D17 | Pin 21 | GPIO 0 (ID_SD) | `0` |
| 20 | A2/D2 | Pin 20 | GPIO 25 | `25` |
| 21 | A5/D5 | Pin 23 | GPIO 5 | `5` |
| 22 | A3/D3 | Pin 22 | GPIO 8 (CE0) | `8` |
| 23 | A6/D6 | Pin 25 | GPIO 6 | `6` |
| 24 | A4/D4 | Pin 24 | GPIO 7 (CE1) | `7` |
| 25 | SDA1/D8 | Pin 27 | GPIO 13 (PWM1) | `13` |
| 26 | GND | Pin 26 | GND | — |
| 27 | MISO1/D13 | Pin 29 | GPIO 19 (PCM_FS) | `19` |
| 28 | D18 | Pin 28 | GPIO 16 | `16` |
| 29 | SCL1/D9 | Pin 31 | GPIO 26 | `26` |
| 30 | D16 | Pin 30 | GPIO 12 | `12` |
| 31 | GND | Pin 33 | GND | — |
| 32 | CLK1/D14 | Pin 32 | GPIO 20 | `20` |
| 33 | — | Pin 35 | GPIO 29 | `29` |
| 34 | MOSI1/D12 | Pin 34 | GPIO 21 | `21` |
| 35 | 3.3V | Pin 37 | 3V3 | — |
| 36 | — | Pin 36 | — | — |
| 37 | GND | Pin 39 | GND | — |
| 38 | GND | Pin 38 | GND | — |
| 39 | — | Pin 41 | — | — |
| 40 | — | Pin 40 | — | — |

> **Note:** The RPi Zero is a single-board computer, not an MCU. For Arduino-style development, use the Arduino IDE with the `arduino-pico` or `ArduinoBoardManager` RPi support. Alternatively, use Python with `RPi.GPIO` or `gpiozero`. The RPi Zero is recommended only when camera, HDMI, or Linux-level processing is needed.

---

## Quick Signal Lookup by Function

Use this table to rapidly identify which MOAB signals to use for common tasks:

| Function | MOAB Signals | Notes |
|----------|-------------|-------|
| I2C (primary) | SDA0, SCL0 | Shared bus — address-based, multi-device OK |
| I2C (secondary) | SDA1/D8, SCL1/D9 | Use for bus isolation or second bus |
| SPI (primary) | CLK0, MOSI0, MISO0 | Need separate CS pin per device (use A0–A7) |
| SPI (secondary) | CLK1/D14, MOSI1/D12, MISO1/D13 | Available on Pico, XIAO_1, Zero |
| UART (primary) | TX0, RX0 | Single device per UART |
| UART (secondary) | TX1/D10, RX1/D11 | Available on Pico, XIAO_1, Nano, Zero |
| Analog input | A0/D0 – A7/D7 | ADC channel count varies by MCU |
| Digital GPIO | D15, D16, D17, D18 | Digital-only, no ADC |
| PWM output | Any A0–A7 or D-pin | MCU-dependent; most support PWM on any GPIO |
| LED indicator | A0/D0 – A7/D7 via J49 | Active-HIGH, jumper selectable |
| Button input | A0/D0 – A7/D7 via J85 | Active-LOW, internal pull-up needed |
| Buzzer | A2/D2 or A4/D4 via J88 | Passive, requires PWM (tone()) |
| Potentiometer | A0/D0 or AREF via J66 | 500KΩ, analog read |
