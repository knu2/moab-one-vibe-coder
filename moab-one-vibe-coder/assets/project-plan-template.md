# Project: [Name]

> This template is used by the agent to structure the output for each vibe coding session. Fill in all sections based on the user's project description and the MOAB-ONE reference data.

---

## 1. Project Overview

- **Description:** [What the user wants to build — 1-2 sentences]
- **Key Requirements:**
  - [Sensor/actuator/interface 1]
  - [Sensor/actuator/interface 2]
  - [Communication needs]
  - [Power needs]

---

## 2. Hardware Selection

- **MCU Board:** [Board name] ([Form factor])
- **Framework:** Arduino (C/C++)
- **Reason:** [1-sentence justification for the MCU choice]

### Peripheral Mapping

| # | Component | Module/Part | Connector | MOAB Signals | Jumper Config |
|:-:|-----------|-------------|-----------|-------------|---------------|
| 1 | [name] | [specific part] | [connector ID] | [signals] | [jumpers] |
| 2 | [name] | [specific part] | [connector ID] | [signals] | [jumpers] |
| 3 | [name] | [specific part] | [connector ID] | [signals] | [jumpers] |

### On-Board Peripherals Used

| Peripheral | MOAB Signal | Jumper | Purpose |
|-----------|-------------|--------|---------|
| [LED/Button/Pot/Buzzer] | [signal] | [header] | [purpose] |

---

## 3. Power Configuration

- **Mode:** [1: USB / 2: External 5V / 3: PS-ONE / 4: Combined]
- **Source:** [USB cable / 5V wall adapter / battery pack / PS-ONE + battery]

| Jumper | Setting | Reason |
|--------|---------|--------|
| J18 | [pin positions] | [explanation] |
| J19 | [pin positions] | [explanation] |
| J87 | [pin positions or "no jumper"] | [explanation] |

---

## 4. Wiring Summary

| MOAB Signal | MCU Pin | Connected To | Interface |
|-------------|---------|-------------|-----------|
| [SDA0] | [pin] | [peripheral] | [I2C/SPI/etc.] |
| [SCL0] | [pin] | [peripheral] | [I2C/SPI/etc.] |

---

## 5. Firmware

```cpp
/*
 * [Project Name]
 * MCU: [Board name] (Arduino framework)
 *
 * Connections via MOAB-ONE:
 *   [Peripheral 1] → [Connector] ([Interface]: [MOAB signals] = [MCU pins])
 *   [Peripheral 2] → [Connector] ([Interface]: [MOAB signals] = [MCU pins])
 *
 * Libraries (install via Arduino Library Manager):
 *   - [Library 1]
 *   - [Library 2]
 */

// [Complete working Arduino sketch here]
```

---

## 6. Assembly Steps

1. Mount [MCU board] on MDB socket (align to **[form factor]** label)
2. Set power jumpers: [J18, J19, J87 settings]
3. [Set IO jumpers: J49, J85, J66, J88 as needed]
4. [Mount/connect peripheral 1]
5. [Mount/connect peripheral 2]
6. [Connect power source]

---

## 7. Testing Sequence

1. ✅ Power LEDs D9 (3.3V) and D10 (5V) should light up
2. ✅ Upload firmware and open Serial Monitor at [baud rate]
3. ✅ [Verify peripheral 1 initialization]
4. ✅ [Verify peripheral 2 initialization]
5. ✅ [Verify integrated functionality]

---

## 8. Troubleshooting

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| [symptom] | [cause] | [fix] |
