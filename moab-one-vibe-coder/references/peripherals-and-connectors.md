# MOAB-ONE Peripherals & Connectors Reference

## Connector Overview

MOAB-ONE provides a rich set of peripheral sockets and connectors. The agent should select connectors in this priority order based on ease of use:

1. **Stemma QT / Qwiic** — Smallest, easiest for I2C, hot-swappable
2. **Grove** — Simple 4-pin, versatile (I2C, UART, analog, digital)
3. **Stemma / Gravity 3-pin** — Single analog/digital/PWM signals
4. **Stemma / Gravity 4-pin** — I2C or UART
5. **mikroBUS™** — Feature-rich Click boards with multiple interface options
6. **Arduino Uno Shield** — Full shield ecosystem for complex add-ons
7. **Display Header** — Dedicated display connection
8. **Prototyping Area** — Custom circuits with through-hole components

---

## mikroBUS™ Sockets

MOAB-ONE has **4 mikroBUS™ sockets** compatible with 1800+ MikroE Click boards.

### Socket Positions

| Socket | Location | Configurable | Size Support | Jumper Headers |
|--------|----------|-------------|-------------|---------------|
| 1 (upper-left) | Top edge, left | Yes | S, M | Side jumper headers |
| 2 (upper-right) | Top edge, right | Yes | S, M | Side jumper headers |
| 3 (lower-left) | Bottom edge, left | No (fixed) | S, M, L | — |
| 4 (lower-right) | Bottom edge, right | No (fixed) | S, M, L | — |

> **Warning:** Sockets 1 and 2 overlap with the Arduino Uno Shield socket. Mounting a Shield renders these sockets unusable. Sockets 3 and 4 remain available.

### mikroBUS™ Standard Pin Configuration

Each socket has two 8-pin headers (left and right):

| Pin | Left Header | Right Header |
|:---:|-------------|-------------|
| 1 | AN (Analog) | PWM |
| 2 | RST (Reset) | INT |
| 3 | CS (SPI Chip Select) | RX (UART) |
| 4 | SCK (SPI Clock) | TX (UART) |
| 5 | MISO (SPI Data Out) | SCL (I2C Clock) |
| 6 | MOSI (SPI Data In) | SDA (I2C Data) |
| 7 | +3.3V | +5V |
| 8 | GND | GND |

### MOAB Signal Routing per Socket

| Signal | Socket 1 | Socket 2 | Socket 3 | Socket 4 |
|--------|----------|----------|----------|----------|
| AN | A0/D0_B | A1/D1_B | A2/D2_B | A3/D3_B |
| SDA | SDA0_B | SDA0_B | SDA0_B | SDA0_B |
| SCL | SCL0_B | SCL0_B | SCL0_B | SCL0_B |
| TX | TX0_B | TX0_B | TX0_B | TX1/D10_B |
| RX | RX0_B | RX0_B | RX0_B | RX1/D11_B |
| MOSI | MOSI0_B | MOSI0_B | MOSI0_B | MOSI0_B |
| MISO | MISO0_B | MISO0_B | MISO0_B | MISO0_B |
| SCK | CLK0_B | CLK0_B | CLK0_B | CLK0_B |
| INT | varies | varies | MISO1/D13_B | RX1/D11_B |
| PWM | varies | varies | TX1/D10_B | MISO1/D13_B |

> **Important:** Multiple I2C Click boards can share the same bus (sockets use the same SDA0/SCL0 lines) because I2C is address-based. However, UART and SPI devices on different sockets will conflict unless they use different bus signals.

### Click Board Size Standards

| Size | Dimensions | Compatible Sockets |
|------|-----------|-------------------|
| S (Small) | 28.6 × 25.4 mm | All 4 sockets |
| M (Medium) | 42.9 × 25.4 mm | All 4 sockets |
| L (Large) | 57.15 × 25.4 mm | Sockets 3 and 4 only (lower) |

---

## Arduino Uno Shield Socket

Full Arduino Uno R3-compatible shield socket. Supports hundreds of available shields.

### Pin Mapping

**Header J44 (Power, 8 pins):**

| Pin | Signal | MOAB Connection |
|:---:|--------|----------------|
| 1 | NC/IOREF | Not connected |
| 2 | RESET | RESET |
| 3 | +3V3 | 3V3_OUT |
| 4 | +5V | 5V_OUT |
| 5 | GND | GND |
| 6 | GND | GND |
| 7 | VIN | VIN |

**Header J43 (Analog, 6 pins):**

| Pin | Shield Label | MOAB Signal |
|:---:|-------------|-------------|
| 1 | A0 | A0/D0_B |
| 2 | A1 | A1/D1_B |
| 3 | A2 | A2/D2_B |
| 4 | A3 | A3/D3_B |
| 5 | A4 | A4/D4_B |
| 6 | A5 | A5/D5_B |

**Header J42 (Digital 0–7, 10 pins):**

| Pin | Shield Label | MOAB Signal |
|:---:|-------------|-------------|
| 1 | D0/RX | RX0_B |
| 2 | D1/TX | TX0_B |
| 3 | D2 | D15 |
| 4 | D3 | D16 |
| 5 | D4 | D17 |
| 6 | D5 | D18 |
| 7 | D6 | A6/D6_B |
| 8 | D7 | A7/D7_B |

**Header J45 (Digital 8–13 + I2C, 10 pins):**

| Pin | Shield Label | MOAB Signal |
|:---:|-------------|-------------|
| 1 | D8 | SDA1/D8_B |
| 2 | D9 | SCL1/D9_B |
| 3 | D10 | TX1/D10_B |
| 4 | D11/MOSI | MOSI0_B |
| 5 | D12/MISO | MISO0_B |
| 6 | D13/SCK | CLK0_B |
| 7 | GND | GND |
| 8 | AREF | AREF |
| 9 | SDA | SDA0_B |
| 10 | SCL | SCL0_B |

---

## Stemma / Gravity 4-pin (JST-PH)

One Stemma connector (J47) and one Gravity connector (J55) using 2mm-pitch JST-PH 4-pin connectors.

### Pin Configurations

| Pin | Stemma (I2C only) | Gravity (I2C mode) | Gravity (UART mode) |
|:---:|:------------------:|:------------------:|:-------------------:|
| 1 | SCL | SDA | TX |
| 2 | SDA | SCL | RX |
| 3 | VCC | GND | GND |
| 4 | GND | VCC | VCC |

> **Note:** Stemma and Gravity use the same JST-PH physical connector but have different pinouts. They are NOT interchangeable without checking pin order.

### Jumper Configuration

**Stemma (J47):**
- **J50** — VCC selection: Short pins 1-2 for 3.3V, pins 2-3 for 5V
- Signal: Fixed to SDA0_B, SCL0_B

**Gravity (J55):**
- **J56** — VCC selection: Short pins 1-2 for 3.3V, pins 2-3 for 5V
- **J57** — Signal pin 1: Short to SDA0_B (I2C) or TX0_B (UART)
- **J58** — Signal pin 2: Short to SCL0_B (I2C) or RX0_B (UART)

---

## Stemma / Gravity 3-pin (JST-PH)

Two 3-pin connectors (J53, J60) for single analog, digital, or PWM signal peripherals.

### Pin Configuration (same for both Stemma and Gravity 3-pin)

| Pin | Signal |
|:---:|--------|
| 1 | Signal (analog, digital, or PWM) |
| 2 | VCC |
| 3 | GND |

### Jumper Configuration

**Connector J53:**
- **J48** — Signal selection: A0/D0_B (pin 1-2) or A1/D1_B (pin 2-3)
- **J54** — VCC selection: 5V_B (pin 1-2) or 3.3V_B (pin 2-3)

**Connector J60:**
- **J59** — Signal selection: A2/D2_B (pin 1-2) or A3/D3_B (pin 2-3)
- **J61** — VCC selection: 5V_B (pin 1-2) or 3.3V_B (pin 2-3)

---

## Stemma QT / Qwiic (JST-SH)

Four 1mm-pitch JST-SH 4-pin connectors for I2C peripherals. These are the easiest connectors to use for I2C devices.

### Pin Configuration (standard for all 4 connectors)

| Pin | Signal |
|:---:|--------|
| 1 | GND |
| 2 | VCC |
| 3 | SDA |
| 4 | SCL |

### Connector Details

| Connector | I2C Bus Options | VCC Jumper | Signal Jumper |
|-----------|----------------|------------|--------------|
| J62 | I2C0 or I2C1 | J67 (3.3V or 5V) | J63 (SDA0 or SDA1), J67 (SCL0 or SCL1) |
| J68 | I2C0 or I2C1 | J67 (3.3V or 5V) | J72 (SDA0 or SDA1), J67 (SCL0 or SCL1) |
| J76 | I2C0 only (fixed) | J77 (3.3V or 5V) | — |
| J82 | I2C0 only (fixed) | J83 (3.3V or 5V) | — |

> **Default:** Leave signal jumpers at I2C0 positions. Use J76 and J82 (fixed to I2C0) for the simplest plug-and-play setup.

> **Qwiic boards** operate at 3.3V only. Stemma QT boards accept 3V–5V. When mixing Qwiic and Stemma QT devices on the same bus, set VCC to 3.3V.

---

## Grove (4-pin, proprietary connector)

Two Grove connectors (J64, J73) with flexible signal routing.

### Pin Configuration

| Pin | Signal |
|:---:|--------|
| 1 | Signal 1 (configurable) |
| 2 | Signal 2 (configurable) |
| 3 | VCC |
| 4 | GND |

### Jumper Configuration

**Connector J64:**
- **J65** — VCC: 5V_B (pin 1-2) or 3.3V_B (pin 2-3)
- **J69** — Pin 1: A0/D0_B, RX0_B, or SDA0_B
- **J70** — Pin 2: A1/D1_B, TX0_B, or SCL0_B

**Connector J73:**
- **J75** — VCC: 5V_B (pin 1-2) or 3.3V_B (pin 2-3)
- **J78/J79** — Pin 1: A2/D2_B, RX1/D11_B, or SDA0_B
- **J80** — Pin 2: A3/D3_B, TX1/D10_B, or SCL0_B

> **Mechanical Warning:** Grove connectors look similar to JST-PH (Stemma/Gravity) but are NOT mechanically compatible. Do not force a Grove cable into a Stemma/Gravity connector or vice versa.

---

## Display Header (J20/J21)

Dedicated header for display modules.

- **J20** — 20-pin single-row female header for the display module
- **J21** — 40-pin double-row female header for jumper bridge configuration

### Default Wiring (straight-through jumpers)

Compatible with Adafruit's TFT-LCD displays in SPI mode:
- Adafruit 3.5" TFT-LCD (Product ID: 5846)
- Adafruit 2.8" TFT-LCD (Product ID: 2090)

| Display Pin | Function | MOAB Signal |
|:-----------:|----------|-------------|
| 1 | GND | GND |
| 2 | VIN | 5V_B or 3V3_B (via J28) |
| 3 | VOUT | — |
| 4 | CLK | CLK0_B |
| 5 | MISO | MISO0_B |
| 6 | MOSI | MOSI0_B |
| 7 | CS | A6/D6_B |
| 8 | D/C | A4/D4_B |
| 9 | RST | A0/D0_B |
| 10 | LITE | MISO1/D13_B |
| 11 | GND | GND |
| 12 | IRQ | — |
| 13 | SDA (touch) | SDA0_B |
| 14 | SCL (touch) | SCL0_B |
| 15 | IM3 | SDA1/D8_B |
| 16 | IM2 | SCL1/D9_B |
| 17 | IM1 | TX1/D10_B |
| 18 | IM0 | MOSI1/D12_B |
| 19 | CCS | — |
| 20 | CD | — |

> **For other displays** (OLEDs, e-paper, custom): Remove the jumper connectors from J21 and use jumper wires to reconfigure the signal routing between J20 (display side) and the MCU signals.
