# MOAB-ONE On-Board Peripherals Reference

## LEDs (8×)

Eight LEDs of different colors driven directly from MOAB signals A0/D0 through A7/D7.

### LED Specifications

| LED | Color | Series Resistor | MOAB Signal |
|:---:|-------|:--------------:|-------------|
| D1 | Red | R3 = 820Ω | A0/D0_B |
| D2 | Red | R4 = 820Ω | A1/D1_B |
| D3 | Yellow | R5 = 820Ω | A2/D2_B |
| D4 | Yellow | R6 = 820Ω | A3/D3_B |
| D5 | Yellow | R7 = 1.5KΩ | A4/D4_B |
| D6 | Green | R8 = 1.5KΩ | A5/D5_B |
| D7 | Green | R9 = 1.5KΩ | A6/D6_B |
| D8 | Green | R10 = 1.5KΩ | A7/D7_B |

### Behavior
- **Active-HIGH:** Write `HIGH` to turn ON, `LOW` to turn OFF.
- **Low current draw:** A few milliamps each — safe to drive directly from any GPIO.

### Jumper Configuration (J49)

J49 is a 2×8 jumper header. For each signal pair (a-side = MCU signal, b-side = LED):
- **Jumper installed (a→b):** LED connected to MCU signal
- **Jumper removed:** LED disconnected

### Arduino Code

```cpp
// Example: Blink LED D1 (Red) on MOAB signal A0/D0
const int LED_RED_1 = A0;  // MOAB: A0/D0 → LED D1 via J49

void setup() {
  pinMode(LED_RED_1, OUTPUT);
}

void loop() {
  digitalWrite(LED_RED_1, HIGH);  // LED ON
  delay(500);
  digitalWrite(LED_RED_1, LOW);   // LED OFF
  delay(500);
}
```

---

## Buttons (8×)

Eight tactile push-buttons connected to the same A0/D0 through A7/D7 signals via a separate jumper header.

### Button Specifications

| Button | MOAB Signal |
|:------:|-------------|
| SW1 | A0/D0_B |
| SW2 | A1/D1_B |
| SW3 | A2/D2_B |
| SW4 | A3/D3_B |
| SW5 | A4/D4_B |
| SW6 | A5/D5_B |
| SW7 | A6/D6_B |
| SW8 | A7/D7_B |

### Behavior
- **Active-LOW:** Reads `LOW` when pressed, `HIGH` when released.
- **No external pull-up resistors:** The MCU's internal pull-up MUST be enabled via firmware.

### Jumper Configuration (J85)

J85 connects buttons to the MCU signals. Works independently of J49 (LEDs), so you can have:
- A signal connected to BOTH an LED and a button (read button, see LED feedback)
- A signal connected to only LED or only button
- A signal connected to neither (for use with external peripherals)

### Arduino Code

```cpp
// Example: Read button SW1 on MOAB signal A0/D0
const int BUTTON_1 = A0;  // MOAB: A0/D0 → SW1 via J85

void setup() {
  pinMode(BUTTON_1, INPUT_PULLUP);  // CRITICAL: Enable internal pull-up
  Serial.begin(115200);
}

void loop() {
  if (digitalRead(BUTTON_1) == LOW) {
    Serial.println("Button 1 pressed!");
    delay(200);  // Simple debounce
  }
}
```

### Typical LED/Button Configurations

**Config A — 4 buttons + 4 LEDs:**
- A0/D0–A3/D3 → Buttons (J85 jumpers installed, J49 jumpers removed)
- A4/D4–A7/D7 → LEDs (J49 jumpers installed, J85 jumpers removed)

**Config B — 8 LEDs, no buttons:**
- All J49 jumpers installed, all J85 jumpers removed

**Config C — All signals free for external peripherals:**
- All J49 and J85 jumpers removed

---

## Potentiometer (1×)

Single-turn 500KΩ analog potentiometer for variable voltage input.

### Specifications

| Parameter | Value |
|-----------|-------|
| Resistance | 500KΩ |
| End-stop resistors | R11, R12 = 100Ω each |
| Output range | ~0V to ~VCC (limited by end-stop resistors) |

### Jumper Configuration

**J66 (Signal routing, 3-pin):**

| Pins Shorted | Connection |
|:------------:|------------|
| 1-2 | Wiper → A0/D0_B |
| 2-3 | Wiper → AREF |

**J86 (VCC selection, 2-pin):**

| Pins Shorted | VCC Level |
|:------------:|-----------|
| 1-2 | 5V_B |
| 2-3 | 3.3V_B |

> **Note:** When routing the pot to A0/D0, make sure J49 and J85 jumpers are removed for A0/D0 to avoid conflicts with LEDs/buttons.

### Arduino Code

```cpp
// Example: Read potentiometer on A0/D0
const int POT_PIN = A0;  // MOAB: A0/D0 via J66 (pins 1-2)

void setup() {
  Serial.begin(115200);
}

void loop() {
  int value = analogRead(POT_PIN);
  float voltage = value * (3.3 / 1023.0);  // Adjust for your ADC resolution
  Serial.print("Pot value: ");
  Serial.print(value);
  Serial.print(" (");
  Serial.print(voltage, 2);
  Serial.println("V)");
  delay(100);
}
```

---

## Buzzer (1×)

Passive buzzer driven by an NPN transistor (S8050 / Q1) for tone generation.

### Specifications

| Parameter | Value |
|-----------|-------|
| Type | Passive (requires PWM signal) |
| Driver | NPN transistor Q1 (S8050) via R13 = 1.5KΩ |
| Flyback diode | D11 (1N4148WS) |
| Power | 3V3_B rail |

### Jumper Configuration (J88, 3-pin)

| Pins Shorted | MCU Signal |
|:------------:|------------|
| 1-2 | A2/D2_B |
| 2-3 | A4/D4_B |

> **Note:** When using the buzzer, remove the corresponding J49/J85 jumpers for the chosen signal (A2/D2 or A4/D4) to avoid LED/button interference.

### Arduino Code

```cpp
// Example: Play a melody on the buzzer
const int BUZZER_PIN = A2;  // MOAB: A2/D2 via J88 (pins 1-2)

// Note frequencies (Hz)
#define NOTE_C4  262
#define NOTE_D4  294
#define NOTE_E4  330
#define NOTE_G4  392

void setup() {
  // Play a startup melody
  int melody[] = {NOTE_C4, NOTE_E4, NOTE_G4, NOTE_C4};
  int durations[] = {200, 200, 200, 400};

  for (int i = 0; i < 4; i++) {
    tone(BUZZER_PIN, melody[i], durations[i]);
    delay(durations[i] + 50);
  }
  noTone(BUZZER_PIN);
}

void loop() {
  // Your code here
}
```

---

## Prototyping Area

A 30×13 grid of 0.1"-spaced through-holes for custom circuits.

### Grid Layout

- **Columns:** 1–30 (horizontal)
- **Rows:** A–M (vertical, A at bottom, M at top)
- **Connections:** Breadboard-style — groups of holes are shorted together as indicated by white rectangular outlines on the PCB

### Power Rails (top row)

Connected to 3V3_B, GND, and 5V_B via header J51.

### Signal Row (Row A, bottom)

Row A provides direct access to all peripheral-side MOAB signals:

| Column | Signal | Column | Signal |
|:------:|--------|:------:|--------|
| 1 | SDA0_B | 16 | SDA1/D8_B |
| 2 | SCL0_B | 17 | SCL1/D9_B |
| 3 | TX0_B | 18 | TX1/D10_B |
| 4 | RX0_B | 19 | RX1/D11_B |
| 5 | MOSI0_B | 20 | MOSI1/D12_B |
| 6 | MISO0_B | 21 | MISO1/D13_B |
| 7 | CLK0_B | 22 | CLK1/D14_B |
| 8 | A0/D0_B | 23 | D15 |
| 9 | A1/D1_B | 24 | D16 |
| 10 | A2/D2_B | 25 | D17 |
| 11 | A3/D3_B | 26 | D18 |
| 12 | A4/D4_B | 27 | AREF |
| 13 | A5/D5_B | 28 | 3V3_B |
| 14 | A6/D6_B | 29 | 5V_B |
| 15 | A7/D7_B | 30 | — |

### Usage Tips

- Use rows B–M for placing through-hole components (resistors, capacitors, transistors, DIP ICs).
- Connect component leads to row A holes to interface with MCU signals.
- Use the top power rail for VCC and GND connections.
- The prototyping area is ideal for voltage dividers, signal conditioning circuits, relay drivers, and custom sensor interfaces.
