---
name: moab-one-vibe-coder
description: >
  Generate complete hardware project implementations using the MOAB-ONE universal
  carrier board. Use when a user describes an electronics project idea, wants to
  prototype with microcontrollers, needs help selecting MCU boards and peripherals,
  requires wiring and jumper configuration instructions, or wants Arduino firmware
  code generated. Covers Adafruit Feather, Arduino Nano, Raspberry Pi Pico, Seeed
  Studio XIAO, and Raspberry Pi Zero MCU form factors with mikroBUS Click boards,
  Arduino Shields, Stemma/Gravity, Stemma QT/Qwiic, and Grove peripherals.
  Use this skill for embedded systems prototyping, IoT projects, robotics, sensor
  networks, data loggers, and student lab exercises.
license: Apache-2.0
metadata:
  author: knu2
  version: "1.0"
---

# MOAB-ONE Vibe Coder

## What is MOAB-ONE

MOAB-ONE is a universal carrier and expansion board (170 mm × 108 mm) for rapid embedded prototyping. It accepts **five MCU development board form factors** (Feather, Nano, Pico, XIAO/QT Py, RPi Zero) in a single Multi Development Board (MDB) socket, and provides standardized signal routing to peripherals — so changing your MCU board does not require rewiring. The board includes four mikroBUS™ sockets, an Arduino Uno Shield socket, multiple Stemma/Gravity/Qwiic/Grove connectors, on-board LEDs, buttons, a potentiometer, a buzzer, a display header, a prototyping area, and flexible power management with an optional PS-ONE DC-DC supply module.

## Vibe Coding Workflow

When a user describes a project idea, follow these four steps in order. Produce the output using the template in `assets/project-plan-template.md`.

### Step 1 — Understand the Project

1. Parse the user's natural-language project description.
2. Identify **what the project needs to sense** (temperature, light, motion, distance, etc.).
3. Identify **what the project needs to actuate** (motors, LEDs, relays, speakers, etc.).
4. Identify **communication interfaces** required (WiFi, Bluetooth, USB serial, SD card, display).
5. Identify **power requirements** (USB-tethered, battery-powered, wall adapter, wide-input field).
6. Ask clarifying questions only if the description is genuinely ambiguous. Prefer sensible defaults over interrogating the user.

### Step 2 — Design the Hardware Plan

1. **Select the MCU board** using the decision guide below.
2. **Map each peripheral** to a MOAB-ONE connector type. Consult `references/peripherals-and-connectors.md` for connector options and jumper settings.
3. **Resolve pin conflicts** — verify no two peripherals share the same MOAB signal. Use `references/pin-mappings.md` for exact pin assignments.
4. **Configure power** — select the power source and jumper positions. Use `references/power-configurations.md`.
5. **Plan on-board peripheral usage** — decide which LEDs, buttons, pot, or buzzer to use. Use `references/on-board-peripherals.md`.

### Step 3 — Generate Firmware

1. Default to **Arduino (C/C++)** unless the user requests another framework.
2. Use the standard Arduino pin identifiers for the selected MCU board (see pin mappings reference).
3. Structure the code with clear `setup()` and `loop()` functions.
4. Include library `#include` directives with exact library names installable via Arduino Library Manager.
5. Add comments explaining which MOAB signal each pin constant maps to.
6. Include a serial output section for debugging and verification.

### Step 4 — Guide Assembly & Testing

1. Provide **ordered physical assembly steps**: mount MCU → set power jumpers → set IO jumpers → mount peripherals → connect cables → apply power.
2. Provide a **testing sequence** that verifies one subsystem at a time (power LEDs → serial connection → each peripheral individually → integrated test).
3. Warn about any gotchas specific to the chosen configuration.

---

## MCU Board Selection Guide

Use this decision tree. Pick the **first match** from top to bottom:

| Requirement | Recommended Board | Form Factor |
|-------------|-------------------|-------------|
| Camera or HDMI output needed | Raspberry Pi Zero 2 W | RPi Zero |
| WiFi + compact size | XIAO ESP32-S3 or ESP32-C3 | XIAO |
| WiFi + many GPIO pins | Raspberry Pi Pico 2 W | Pico |
| Bluetooth Low Energy | Adafruit Feather nRF52840 | Feather |
| Maximum Arduino ecosystem compatibility | Arduino Nano 33 BLE Sense Rev2 | Nano |
| Ultra-compact, minimal cost | Seeed Studio XIAO SAMD21 | XIAO |
| Need 2 MCUs simultaneously | Two XIAO boards (XIAO_0 + XIAO_1) | XIAO |
| General purpose / student learning | Raspberry Pi Pico 2 | Pico |
| Battery-powered with built-in charging | Any Adafruit Feather | Feather |

> **Default:** When no strong constraint exists, recommend the **Raspberry Pi Pico 2** (or Pico 2 W if wireless is needed). It offers the best balance of GPIO count, cost, documentation, and Arduino support.

---

## Peripheral Matching Quick Reference

| Project Need | Recommended Connector | Example Boards/Modules |
|-------------|----------------------|----------------------|
| I2C sensor (temp, humidity, pressure, IMU) | Stemma QT / Qwiic (J62, J68, J76, J82) | BME280, BMP390, MPU6050, SHT40 |
| I2C sensor (alternative) | Grove (J64, J73) in I2C mode | Seeed Grove sensors |
| Analog sensor (soil moisture, light, gas) | Stemma/Gravity 3-pin (J53, J60) | Analog LDR, soil moisture |
| UART module (GPS, fingerprint, voice) | Gravity 4-pin (J55) in UART mode | GPS NEO-6M, fingerprint scanner |
| Feature-rich add-on (Click boards) | mikroBUS™ (4 sockets) | 1800+ MikroE Click boards |
| Motor driver or complex shield | Arduino Uno Shield socket | L293D motor shield, relay shield |
| Display (TFT-LCD with touch) | Display header (J20/J21) | Adafruit 2.8" or 3.5" TFT-LCD |
| Display (small OLED) | Display header (J20) or Stemma QT | SSD1306 0.96" OLED (I2C) |
| Custom circuit | Prototyping area (30×13 grid) | Any through-hole components |

> **Priority order for I2C devices:** Stemma QT/Qwiic first (easiest, hot-swappable), then Grove, then mikroBUS.

---

## Arduino Firmware Generation Rules

1. **Pin definitions:** Always define pin constants at the top of the sketch with comments referencing the MOAB signal name:
   ```cpp
   // MOAB Signal: A0/D0 → connected to LED via J49
   const int LED_PIN = A0;
   ```

2. **I2C:** Use `Wire.begin()` for I2C0 (SDA0/SCL0 — default on all supported boards). For I2C1 on boards that support it (Pico, XIAO_1), use `Wire1.begin()`.

3. **SPI:** Use the default `SPI` object for SPI0 (CLK0/MOSI0/MISO0).

4. **UART:** Use `Serial` for USB debug output. Use `Serial1` for UART0 (TX0/RX0) peripheral communication.

5. **Buttons:** Configure with internal pull-up:
   ```cpp
   pinMode(BUTTON_PIN, INPUT_PULLUP);
   // Read LOW when pressed (active-low)
   ```

6. **Buzzer:** Drive with `tone()`:
   ```cpp
   // MOAB Signal: A2/D2 or A4/D4 via J88
   tone(BUZZER_PIN, 1000, 500); // 1kHz, 500ms
   ```

7. **Potentiometer:** Read with `analogRead()`:
   ```cpp
   // MOAB Signal: A0/D0 via J66
   int value = analogRead(POT_PIN);
   ```

---

## Gotchas

- **Arduino Shield overlaps upper mikroBUS:** Mounting an Arduino Shield renders mikroBUS sockets 1 and 2 (upper pair) unusable. Plan for the lower sockets 3 and 4 if using both.
- **XIAO slots are NOT identical:** XIAO_0 connects to bus 0 signals (SDA0, SCL0, TX0, RX0, SPI0). XIAO_1 connects to bus 1 signals (SDA1, SCL1, TX1, RX1, SPI1). This matters for peripheral routing.
- **Buttons have NO external pull-up resistors:** The MCU's internal pull-up must be enabled in firmware (`INPUT_PULLUP`). Forgetting this results in floating inputs.
- **LEDs are active-HIGH:** Write `HIGH` to turn on. They draw only a few mA each and can be driven directly from GPIO.
- **Power conflict risk:** Never short together two power outputs. When using external 5V DC without PS-ONE, do NOT connect the MCU via USB simultaneously. When using PS-ONE, do NOT install jumpers on J87.
- **Buzzer is passive:** Requires a PWM signal (use `tone()`). Sending a constant HIGH will produce no sound.
- **Potentiometer range:** Has 100Ω end-stop resistors (R11, R12). The output voltage won't reach exactly 0V or VCC.
- **Display header default wiring:** Straight-through jumper connections on J21 are pre-configured for Adafruit's 3.5"/2.8" TFT-LCD in SPI mode with I2C capacitive touch. Other displays need jumper wire reconfiguration.

---

## Reference Files

For detailed technical data, load these files as needed:

- **[Pin mappings](references/pin-mappings.md)** — Complete MOAB signal to MCU pin mapping for all 5 form factors.
- **[Peripherals & connectors](references/peripherals-and-connectors.md)** — All connector types, jumper configurations, and pin assignments.
- **[Power configurations](references/power-configurations.md)** — Power source options, jumper settings, and PS-ONE specifications.
- **[On-board peripherals](references/on-board-peripherals.md)** — LEDs, buttons, potentiometer, buzzer, and prototyping area details.
- **[Example projects](references/example-projects.md)** — 3 fully worked project implementations to use as patterns.
