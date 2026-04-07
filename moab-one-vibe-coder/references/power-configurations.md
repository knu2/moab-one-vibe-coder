# MOAB-ONE Power Configurations Reference

## Power Architecture

MOAB-ONE separates power into two domains:

| Domain | Rails | Fed By |
|--------|-------|--------|
| **MCU side** | 3V3_MCU, 5V_MCU | MCU board's USB connection or external source |
| **Peripheral side** | 3V3_B, 5V_B | MCU rails, external DC, or PS-ONE |

Three jumper headers control how power is routed between these domains:

| Jumper | Pins | Function |
|--------|------|----------|
| **J18** | 4-pin | Bridges 5V between MCU and peripheral domains |
| **J19** | 3-pin | Bridges 3.3V between MCU and peripheral domains |
| **J87** | 3-pin | Selects external voltage source (5V_EXT or 3V3_EXT) |

**Power indicators:** LED D9 (Red) = 3.3V present, LED D10 (Red) = 5V present.

---

## Configuration 1: USB Power from MCU Board

**Use when:** The project is USB-tethered to a PC/laptop for development and low-power peripherals.

**Setup:**
- MCU board connected via USB to PC/laptop
- No external power source connected
- PS-ONE NOT installed

**Jumper Settings:**

| Jumper | Setting | Description |
|--------|---------|-------------|
| J18 | Pins 1-2 shorted | Route 5V from MCU USB to peripherals |
| J19 | Pins 1-2 shorted | Route 3.3V from MCU LDO to peripherals |
| J87 | No jumper | Not used |

**Current Limits:**
- 5V: Limited by USB source (typically 500 mA from USB 2.0, up to 900 mA from USB 3.0)
- 3.3V: Limited by MCU board's LDO (varies by board, typically 150–600 mA)

> **Warning:** This is the lowest-power option. High-current peripherals (motors, large displays, many LEDs) may cause the LDO to overheat or brownout.

---

## Configuration 2: External 5V DC Input

**Use when:** Higher current is needed, or the project runs standalone without a USB connection.

**Setup:**
- 5V wall adapter or bench supply connected to J15 (barrel jack) or J16 (JST-PH 2-pin)
- Toggle switch SW9 in ON position
- PS-ONE NOT installed
- MCU board NOT connected via USB

**Jumper Settings:**

| Jumper | Setting | Description |
|--------|---------|-------------|
| J18 | Pins 1-2 AND 3-4 shorted | Route external 5V to both MCU and peripheral sides |
| J19 | Pins 1-2 shorted | Route 3.3V from MCU's LDO to peripherals |
| J87 | Pins 1-2 shorted | Select 5V external source |

**Input Connectors:**
- **J15** — 2.1mm barrel jack (center-positive)
- **J16** — 2-pin JST-PH connector
- **J17** — Series jumper for current measurement or external switch

> **Critical:** Do NOT connect the MCU board via USB while using external 5V input. This creates a power conflict between the USB 5V and the external 5V rails.

---

## Configuration 3: PS-ONE DC-DC Power Supply

**Use when:** Wide-input voltage flexibility is needed (battery packs, solar, automotive) or 12V output is required.

### PS-ONE Specifications

| Parameter | Min | Typ | Max | Unit |
|-----------|:---:|:---:|:---:|:----:|
| Input Voltage | 2.8 | — | 16 | V |
| 5V Output Current | — | — | 2 | A |
| 3.3V Output Current | — | — | 1 | A |
| 12V Output Current | — | — | 300 | mA |

### PS-ONE Pinout

| Pin | Left Header (J12) | Right Header (J13) |
|:---:|-------------------|-------------------|
| 1 | GND | GND |
| 2 | GND | 3V OUT |
| 3 | VIN | 5V OUT |
| 4 | VIN | 12V OUT |
| 5 | 12V_EN | PG (Power Good) |

### Compatible Power Sources

- 2–10 series 1.5V alkaline batteries (3V–15V)
- 1–4 series 3.6V Li-Ion cells (3.6V–14.4V)
- 9V battery (primary or rechargeable)
- 5V USB power bank
- Bench supply or wall adapter (2.8V–16V DC)
- Solar panel (with appropriate voltage)

### Setup

1. **Remove** any jumpers from J87 before mounting PS-ONE.
2. Mount PS-ONE board on headers J12 and J13.
3. If 12V output is needed, install jumper on PS-ONE's on-board header. Access 12V via J14 (JST-PH 2-pin).
4. Do NOT connect MCU via USB.

**Jumper Settings:**

| Jumper | Setting | Description |
|--------|---------|-------------|
| J18 | Pins 1-2 AND 3-4 shorted | Route PS-ONE 5V to both MCU and peripheral sides |
| J19 | Pins 2-3 shorted | Route PS-ONE 3.3V to peripherals |
| J87 | No jumper | Must be empty when PS-ONE is installed |

---

## Configuration 4: Combined Power Sources

**Use when:** MCU is USB-connected to a PC for development, but peripherals need more power from an external source.

### Example: USB + PS-ONE

MCU board powered via USB, peripherals powered by PS-ONE from an external battery.

| Jumper | Setting | Description |
|--------|---------|-------------|
| J18 | Pins 3-4 shorted only | External 5V to peripherals only (MCU uses USB) |
| J19 | Pins 2-3 shorted | PS-ONE 3.3V to peripherals |
| J87 | No jumper | PS-ONE installed |

### Example: USB + External 3.3V DC

MCU powered via USB, peripherals get 3.3V from an external supply.

| Jumper | Setting | Description |
|--------|---------|-------------|
| J18 | Pins 1-2 shorted | MCU 5V to peripherals |
| J19 | Pins 2-3 shorted | External 3.3V to peripherals |
| J87 | Pins 2-3 shorted | Select 3.3V external source |

---

## Power Toggle Switch & Current Measurement

- **SW9** — SPDT toggle switch that connects/disconnects the external DC source from the board.
- **J17** — Series jumper header on the DC_IN line. Can be used to:
  - Measure total board current by connecting an ammeter across J17
  - Connect an external ON/OFF switch
  - Leave shorted with a jumper connector for normal operation

---

## Quick Power Selection Guide

| Scenario | Config | J18 | J19 | J87 |
|----------|--------|-----|-----|-----|
| Desk prototyping via USB | 1 | 1-2 | 1-2 | — |
| Wall adapter standalone | 2 | 1-2 + 3-4 | 1-2 | 1-2 |
| Battery pack (wide input) | 3 | 1-2 + 3-4 | 2-3 | — |
| USB dev + battery peripherals | 4 | 3-4 | 2-3 | — |
| USB dev + external 3.3V | 4 | 1-2 | 2-3 | 2-3 |

> **Default recommendation:** For student projects, start with **Configuration 1** (USB power). Move to PS-ONE only when the project needs to run untethered or requires 12V.
