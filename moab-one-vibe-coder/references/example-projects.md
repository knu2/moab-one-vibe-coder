# MOAB-ONE Example Projects

Three fully worked project implementations demonstrating the vibe coding workflow.

---

## Example 1: Weather Station

**User request:** "I want to build a weather station that measures temperature, humidity, and barometric pressure and displays results on a small OLED screen."

### Step 1 — Project Analysis

| Need | Category | Interface |
|------|----------|-----------|
| Temperature + humidity sensor | Sensing | I2C |
| Barometric pressure sensor | Sensing | I2C |
| OLED display | Output | I2C |
| Data logging (optional) | Storage | — |

**No wireless needed.** Three I2C devices on the same bus. No motors, no high power needs.

### Step 2 — Hardware Plan

**MCU Board:** Raspberry Pi Pico 2 (good ADC, affordable, excellent Arduino support)

**Peripherals:**

| Component | Module | Connector | MOAB Signal | Jumper Config |
|-----------|--------|-----------|-------------|---------------|
| Temp/Humidity | SHT40 (Stemma QT) | J76 | SDA0, SCL0 | J77: pins 1-2 (3.3V) |
| Barometric pressure | BMP390 (Stemma QT) | J82 | SDA0, SCL0 | J83: pins 1-2 (3.3V) |
| OLED display 0.96" | SSD1306 128×64 (Stemma QT) | J62 | SDA0, SCL0 | J67: pins 1-2 (3.3V), J63: SDA0 |
| Status LED | On-board LED D6 (Green) | J49 | A5/D5 | J49: A5 jumper installed |

**Power:** Configuration 1 — USB from MCU board.
- J18: pins 1-2 shorted
- J19: pins 1-2 shorted
- J87: no jumper

### Step 3 — Firmware

```cpp
/*
 * MOAB-ONE Weather Station
 * MCU: Raspberry Pi Pico 2 (Arduino framework)
 *
 * Connections via MOAB-ONE:
 *   SHT40  → Stemma QT J76 (I2C0: SDA0=GP4, SCL0=GP5)
 *   BMP390 → Stemma QT J82 (I2C0: SDA0=GP4, SCL0=GP5)
 *   SSD1306 → Stemma QT J62 (I2C0: SDA0=GP4, SCL0=GP5)
 *   Status LED → On-board D6 via J49 (A5/D5 = GP10)
 *
 * Libraries (install via Arduino Library Manager):
 *   - Adafruit SHT4x
 *   - Adafruit BMP3XX
 *   - Adafruit SSD1306
 *   - Adafruit GFX Library
 */

#include <Wire.h>
#include <Adafruit_SHT4x.h>
#include <Adafruit_BMP3XX.h>
#include <Adafruit_SSD1306.h>

// MOAB pin definitions
const int STATUS_LED = 10;   // MOAB: A5/D5 → GP10 on Pico

// Sensor objects
Adafruit_SHT4x sht40 = Adafruit_SHT4x();
Adafruit_BMP3XX bmp;
Adafruit_SSD1306 display(128, 64, &Wire, -1);

// Update interval
const unsigned long UPDATE_INTERVAL_MS = 2000;
unsigned long lastUpdate = 0;

void setup() {
  Serial.begin(115200);
  pinMode(STATUS_LED, OUTPUT);

  // Initialize I2C (MOAB I2C0: SDA0=GP4, SCL0=GP5)
  Wire.begin();

  // Initialize SHT40
  if (!sht40.begin()) {
    Serial.println("ERROR: SHT40 not found!");
    while (1) {
      digitalWrite(STATUS_LED, !digitalRead(STATUS_LED));
      delay(100);  // Fast blink = error
    }
  }
  sht40.setPrecision(SHT4X_HIGH_PRECISION);
  Serial.println("SHT40 initialized.");

  // Initialize BMP390
  if (!bmp.begin_I2C()) {
    Serial.println("ERROR: BMP390 not found!");
    while (1) {
      digitalWrite(STATUS_LED, !digitalRead(STATUS_LED));
      delay(100);
    }
  }
  bmp.setTemperatureOversampling(BMP3_OVERSAMPLING_8X);
  bmp.setPressureOversampling(BMP3_OVERSAMPLING_4X);
  Serial.println("BMP390 initialized.");

  // Initialize OLED
  if (!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
    Serial.println("ERROR: SSD1306 not found!");
    while (1) {
      digitalWrite(STATUS_LED, !digitalRead(STATUS_LED));
      delay(100);
    }
  }
  display.clearDisplay();
  display.setTextSize(1);
  display.setTextColor(SSD1306_WHITE);
  display.setCursor(0, 0);
  display.println("Weather Station");
  display.println("Initializing...");
  display.display();
  Serial.println("OLED initialized.");

  delay(1000);
  digitalWrite(STATUS_LED, HIGH);  // Solid = ready
}

void loop() {
  if (millis() - lastUpdate < UPDATE_INTERVAL_MS) return;
  lastUpdate = millis();

  // Read SHT40
  sensors_event_t humidity_event, temp_event;
  sht40.getEvent(&humidity_event, &temp_event);
  float temperature = temp_event.temperature;
  float humidity = humidity_event.relative_humidity;

  // Read BMP390
  bmp.performReading();
  float pressure = bmp.pressure / 100.0;  // Pa to hPa

  // Serial output
  Serial.print("Temp: "); Serial.print(temperature, 1); Serial.print(" °C  ");
  Serial.print("Hum: "); Serial.print(humidity, 1); Serial.print(" %  ");
  Serial.print("Press: "); Serial.print(pressure, 1); Serial.println(" hPa");

  // OLED display
  display.clearDisplay();
  display.setTextSize(1);
  display.setCursor(0, 0);
  display.println("=== WEATHER STATION ===");
  display.println();
  display.setTextSize(1);
  display.print("Temp:     "); display.print(temperature, 1); display.println(" C");
  display.print("Humidity: "); display.print(humidity, 1); display.println(" %");
  display.print("Pressure: "); display.print(pressure, 1); display.println(" hPa");
  display.display();

  // Toggle LED to show activity
  digitalWrite(STATUS_LED, !digitalRead(STATUS_LED));
}
```

### Step 4 — Assembly & Testing

**Assembly order:**
1. Mount Pico 2 on MDB socket (align to **Pico** label)
2. Set power jumpers: J18 pins 1-2, J19 pins 1-2
3. Install J49 jumper for A5/D5 (LED D6)
4. Connect SHT40 to Stemma QT J76 with cable. Set J77 to 3.3V.
5. Connect BMP390 to Stemma QT J82 with cable. Set J83 to 3.3V.
6. Connect SSD1306 OLED to Stemma QT J62 with cable. Set J67 to 3.3V, J63 to SDA0.
7. Connect Pico 2 to PC via USB cable.

**Test sequence:**
1. ✅ Power LEDs D9 (3.3V) and D10 (5V) should light up
2. ✅ Upload firmware, open Serial Monitor at 115200 baud
3. ✅ Verify "SHT40 initialized" message
4. ✅ Verify "BMP390 initialized" message
5. ✅ Verify "OLED initialized" message
6. ✅ Check serial output shows reasonable temperature, humidity, pressure values
7. ✅ Verify OLED displays readings
8. ✅ Green status LED D6 should toggle every 2 seconds

---

## Example 2: Line-Following Robot

**User request:** "Help me build a robot car with 4 DC motors and an IR line follower sensor."

### Step 1 — Project Analysis

| Need | Category | Interface |
|------|----------|-----------|
| 4× DC motor control | Actuation | Motor shield (PWM + direction) |
| IR line follower sensor | Sensing | Digital or Analog |
| Motor power supply | Power | External (batteries) |

**Key constraint:** L293D motor shield uses Arduino Uno shield form factor → uses the Shield socket. This blocks upper mikroBUS sockets 1 and 2.

### Step 2 — Hardware Plan

**MCU Board:** Arduino Nano 33 BLE Sense Rev2 (native Arduino compatibility, good for motor shield)

**Peripherals:**

| Component | Module | Connector | MOAB Signal | Notes |
|-----------|--------|-----------|-------------|-------|
| L293D Motor Shield | Arduino Shield | Shield socket (J42–J45) | Multiple | Uses D3–D12 for motor control |
| IR Line Follower | Grove Digital | J64 (Grove) | A0/D0_B, A1/D1_B | J65: 5V, J69/J70: A0/A1 |
| Status LEDs | On-board D1, D2 | J49 | A0/D0, A1/D1 | Removed — conflict with IR sensor |

**Power:** Configuration 3 — PS-ONE with 4× AA battery pack (6V).
- PS-ONE mounted on J12/J13
- J18: pins 1-2 and 3-4 shorted
- J19: pins 2-3 shorted
- J87: no jumper

### Step 3 — Firmware

```cpp
/*
 * MOAB-ONE Line-Following Robot
 * MCU: Arduino Nano 33 BLE Sense Rev2
 *
 * Connections via MOAB-ONE:
 *   L293D Motor Shield → Arduino Shield socket
 *     Motor 1 (Front-Left):  M1 terminals
 *     Motor 2 (Front-Right): M2 terminals
 *     Motor 3 (Rear-Left):   M3 terminals
 *     Motor 4 (Rear-Right):  M4 terminals
 *   IR Line Sensor → Grove J64
 *     Left sensor  → A0/D0 (Nano pin A0/D14)
 *     Right sensor → A1/D1 (Nano pin A1/D15)
 *
 * Libraries:
 *   - AFMotor (Adafruit Motor Shield library)
 */

#include <AFMotor.h>

// Motor objects (L293D shield motor ports)
AF_DCMotor motorFL(1);  // Front-Left
AF_DCMotor motorFR(2);  // Front-Right
AF_DCMotor motorRL(3);  // Rear-Left
AF_DCMotor motorRR(4);  // Rear-Right

// MOAB Signal: A0/D0 and A1/D1 via Grove J64
const int IR_LEFT  = A0;   // Nano pin A0
const int IR_RIGHT = A1;   // Nano pin A1

// Speed settings
const int BASE_SPEED = 150;   // 0–255
const int TURN_SPEED = 100;

void setup() {
  Serial.begin(115200);
  pinMode(IR_LEFT, INPUT);
  pinMode(IR_RIGHT, INPUT);

  // Set initial motor speed
  motorFL.setSpeed(BASE_SPEED);
  motorFR.setSpeed(BASE_SPEED);
  motorRL.setSpeed(BASE_SPEED);
  motorRR.setSpeed(BASE_SPEED);

  Serial.println("Line-Following Robot Ready!");
  delay(2000);  // Wait before starting
}

void moveForward() {
  motorFL.run(FORWARD); motorFR.run(FORWARD);
  motorRL.run(FORWARD); motorRR.run(FORWARD);
  motorFL.setSpeed(BASE_SPEED); motorFR.setSpeed(BASE_SPEED);
  motorRL.setSpeed(BASE_SPEED); motorRR.setSpeed(BASE_SPEED);
}

void turnLeft() {
  motorFL.run(BACKWARD); motorFR.run(FORWARD);
  motorRL.run(BACKWARD); motorRR.run(FORWARD);
  motorFL.setSpeed(TURN_SPEED); motorFR.setSpeed(TURN_SPEED);
  motorRL.setSpeed(TURN_SPEED); motorRR.setSpeed(TURN_SPEED);
}

void turnRight() {
  motorFL.run(FORWARD); motorFR.run(BACKWARD);
  motorRL.run(FORWARD); motorRR.run(BACKWARD);
  motorFL.setSpeed(TURN_SPEED); motorFR.setSpeed(TURN_SPEED);
  motorRL.setSpeed(TURN_SPEED); motorRR.setSpeed(TURN_SPEED);
}

void stopMotors() {
  motorFL.run(RELEASE); motorFR.run(RELEASE);
  motorRL.run(RELEASE); motorRR.run(RELEASE);
}

void loop() {
  int leftSensor  = digitalRead(IR_LEFT);   // LOW = line detected
  int rightSensor = digitalRead(IR_RIGHT);  // LOW = line detected

  if (leftSensor == LOW && rightSensor == LOW) {
    // Both sensors on line → go straight
    moveForward();
    Serial.println("FORWARD");
  } else if (leftSensor == LOW && rightSensor == HIGH) {
    // Left sensor on line → turn left
    turnLeft();
    Serial.println("TURN LEFT");
  } else if (leftSensor == HIGH && rightSensor == LOW) {
    // Right sensor on line → turn right
    turnRight();
    Serial.println("TURN RIGHT");
  } else {
    // Both sensors off line → stop and search
    stopMotors();
    Serial.println("SEARCHING...");
  }

  delay(50);  // Control loop rate
}
```

### Step 4 — Assembly & Testing

**Assembly order:**
1. Mount Nano 33 BLE on MDB socket (align to **Nano** label)
2. Mount PS-ONE on J12/J13 (ensure J87 has no jumper)
3. Set power jumpers: J18 pins 1-2 + 3-4, J19 pins 2-3
4. Mount L293D Motor Shield on Arduino Shield socket
5. Connect 4 DC motors to M1–M4 terminals on the shield
6. Set Grove J64 jumpers: J65 to 5V, J69 to A0/D0, J70 to A1/D1
7. Connect IR line sensor to Grove J64 with cable
8. Connect 4×AA battery pack to J15 (barrel jack) or J16 (JST)
9. Turn on SW9

**Test sequence:**
1. ✅ Power LEDs D9 + D10 should light up
2. ✅ Upload firmware (connect Nano via USB — disconnect battery first!)
3. ✅ Open Serial Monitor, verify "Line-Following Robot Ready!"
4. ✅ Hold IR sensor over a dark line — verify serial shows correct sensor readings
5. ✅ Disconnect USB, reconnect battery, flip SW9 to ON
6. ✅ Place robot on a line track — verify it follows the line

---

## Example 3: Bluetooth Environmental Monitor

**User request:** "I need a Bluetooth-connected environmental monitor with a temperature sensor, light sensor, and an OLED display that sends data to my phone."

### Step 1 — Project Analysis

| Need | Category | Interface |
|------|----------|-----------|
| Temperature sensor | Sensing | I2C |
| Light sensor | Sensing | Analog |
| OLED display | Output | I2C |
| Bluetooth LE | Communication | On-board (MCU) |
| Phone notification | Communication | BLE GATT |

**Key requirement:** Bluetooth LE built into the MCU board.

### Step 2 — Hardware Plan

**MCU Board:** Adafruit Feather nRF52840 Express (excellent BLE stack, Arduino-compatible, LiPo charging built-in)

**Peripherals:**

| Component | Module | Connector | MOAB Signal | Jumper Config |
|-----------|--------|-----------|-------------|---------------|
| Temp sensor | SHT40 (Stemma QT) | J76 | SDA0, SCL0 | J77: 3.3V |
| Light sensor | Analog LDR (Stemma 3-pin) | J53 | A0/D0 | J48: A0/D0, J54: 3.3V |
| OLED display | SSD1306 0.96" (Stemma QT) | J82 | SDA0, SCL0 | J83: 3.3V |
| Status LEDs | On-board D6, D7 (Green) | J49 | A5/D5, A6/D6 | J49: A5+A6 jumpers |
| User button | On-board SW1 | J85 | A0/D0 | — (conflict: removed, use SW5 instead) |
| User button | On-board SW5 | J85 | A4/D4 | J85: A4 jumper |

**Power:** Configuration 1 — USB, with optional LiPo battery via Feather's on-board charger.
- J18: pins 1-2, J19: pins 1-2, J87: no jumper

### Step 3 — Firmware

```cpp
/*
 * MOAB-ONE Bluetooth Environmental Monitor
 * MCU: Adafruit Feather nRF52840 Express
 *
 * Connections via MOAB-ONE:
 *   SHT40  → Stemma QT J76 (I2C0: SDA=SDA, SCL=SCL on Feather)
 *   SSD1306 → Stemma QT J82 (I2C0: same bus)
 *   LDR    → Stemma 3-pin J53 (A0/D0 = Feather A0)
 *   LEDs   → D6(Green)=A5, D7(Green)=A6 via J49
 *   Button → SW5=A4 via J85
 *
 * Libraries:
 *   - Adafruit SHT4x
 *   - Adafruit SSD1306
 *   - Adafruit GFX Library
 *   - Adafruit Bluefruit nRF52 (included with board support)
 */

#include <Wire.h>
#include <Adafruit_SHT4x.h>
#include <Adafruit_SSD1306.h>
#include <bluefruit.h>

// MOAB pin definitions (Feather nRF52840)
const int LDR_PIN    = A0;   // MOAB: A0/D0 via Stemma 3-pin J53
const int LED_GREEN1 = A5;   // MOAB: A5/D5 → LED D6 via J49
const int LED_GREEN2 = A6;   // MOAB: A6/D6 → LED D7 via J49
const int BUTTON_PIN = A4;   // MOAB: A4/D4 → Button SW5 via J85

// BLE Environmental Sensing Service (ESS)
BLEService envService = BLEService(0x181A);  // Environmental Sensing
BLECharacteristic tempChar = BLECharacteristic(0x2A6E);  // Temperature
BLECharacteristic humChar  = BLECharacteristic(0x2A6F);  // Humidity
BLECharacteristic lightChar = BLECharacteristic(0x2AFB); // Light level (custom)

// Peripherals
Adafruit_SHT4x sht40;
Adafruit_SSD1306 display(128, 64, &Wire, -1);

unsigned long lastUpdate = 0;
const unsigned long UPDATE_MS = 3000;
bool displayOn = true;

void setup() {
  Serial.begin(115200);
  pinMode(LED_GREEN1, OUTPUT);
  pinMode(LED_GREEN2, OUTPUT);
  pinMode(BUTTON_PIN, INPUT_PULLUP);
  pinMode(LDR_PIN, INPUT);

  // Initialize I2C sensors
  Wire.begin();
  sht40.begin();
  sht40.setPrecision(SHT4X_HIGH_PRECISION);
  display.begin(SSD1306_SWITCHCAPVCC, 0x3C);

  // Initialize BLE
  Bluefruit.begin();
  Bluefruit.setName("MOAB-EnvMon");
  Bluefruit.setTxPower(4);

  // Setup Environmental Sensing Service
  envService.begin();

  tempChar.setProperties(CHR_PROPS_READ | CHR_PROPS_NOTIFY);
  tempChar.setPermission(SECMODE_OPEN, SECMODE_NO_ACCESS);
  tempChar.setFixedLen(2);
  tempChar.begin();

  humChar.setProperties(CHR_PROPS_READ | CHR_PROPS_NOTIFY);
  humChar.setPermission(SECMODE_OPEN, SECMODE_NO_ACCESS);
  humChar.setFixedLen(2);
  humChar.begin();

  lightChar.setProperties(CHR_PROPS_READ | CHR_PROPS_NOTIFY);
  lightChar.setPermission(SECMODE_OPEN, SECMODE_NO_ACCESS);
  lightChar.setFixedLen(2);
  lightChar.begin();

  // Start advertising
  Bluefruit.Advertising.addFlags(BLE_GAP_ADV_FLAGS_LE_ONLY_GENERAL_DISC_MODE);
  Bluefruit.Advertising.addService(envService);
  Bluefruit.Advertising.addName();
  Bluefruit.Advertising.start(0);

  // Startup display
  display.clearDisplay();
  display.setTextSize(1);
  display.setTextColor(SSD1306_WHITE);
  display.setCursor(0, 0);
  display.println("MOAB-ONE EnvMon");
  display.println("BLE: MOAB-EnvMon");
  display.println("Ready!");
  display.display();

  digitalWrite(LED_GREEN1, HIGH);  // Ready indicator
  Serial.println("Environmental Monitor Ready!");
  Serial.println("BLE advertising as: MOAB-EnvMon");
}

void loop() {
  // Button press toggles display
  if (digitalRead(BUTTON_PIN) == LOW) {
    displayOn = !displayOn;
    if (!displayOn) {
      display.clearDisplay();
      display.display();
    }
    delay(300);  // Debounce
  }

  if (millis() - lastUpdate < UPDATE_MS) return;
  lastUpdate = millis();

  // Read sensors
  sensors_event_t hum_event, temp_event;
  sht40.getEvent(&hum_event, &temp_event);
  float temperature = temp_event.temperature;
  float humidity = hum_event.relative_humidity;
  int lightRaw = analogRead(LDR_PIN);
  int lightPercent = map(lightRaw, 0, 1023, 0, 100);

  // Update BLE characteristics
  int16_t bleTemp = (int16_t)(temperature * 100);  // ESS format: 0.01 °C
  uint16_t bleHum = (uint16_t)(humidity * 100);     // ESS format: 0.01 %
  uint16_t bleLight = (uint16_t)lightPercent;
  tempChar.write16(bleTemp);   tempChar.notify16(bleTemp);
  humChar.write16(bleHum);     humChar.notify16(bleHum);
  lightChar.write16(bleLight); lightChar.notify16(bleLight);

  // Serial output
  Serial.printf("T:%.1f°C  H:%.1f%%  L:%d%%\n", temperature, humidity, lightPercent);

  // OLED display
  if (displayOn) {
    display.clearDisplay();
    display.setTextSize(1);
    display.setCursor(0, 0);
    display.println("== ENV MONITOR ==");
    display.println();
    display.print("Temp:  "); display.print(temperature, 1); display.println(" C");
    display.print("Humid: "); display.print(humidity, 1); display.println(" %");
    display.print("Light: "); display.print(lightPercent); display.println(" %");
    display.println();
    display.print("BLE: ");
    display.println(Bluefruit.connected() ? "Connected" : "Advertising");
    display.display();
  }

  // Toggle activity LED
  digitalWrite(LED_GREEN2, !digitalRead(LED_GREEN2));
}
```

### Step 4 — Assembly & Testing

**Assembly order:**
1. Mount Feather nRF52840 on MDB socket (align to **Feather** label)
2. Set power jumpers: J18 pins 1-2, J19 pins 1-2
3. Install J49 jumpers for A5 and A6 (LEDs D6 + D7)
4. Install J85 jumper for A4 (Button SW5)
5. Remove J49 jumper for A0 and J85 jumper for A0 (free A0 for LDR)
6. Connect SHT40 to Stemma QT J76. Set J77 to 3.3V.
7. Connect SSD1306 to Stemma QT J82. Set J83 to 3.3V.
8. Connect LDR Stemma 3-pin breakout to J53. Set J48 to A0/D0, J54 to 3.3V.
9. Connect Feather to PC via USB cable.

**Test sequence:**
1. ✅ Power LEDs D9 + D10 on
2. ✅ Upload firmware, open Serial Monitor at 115200
3. ✅ Verify "Environmental Monitor Ready!" message
4. ✅ Verify sensor readings appear on serial
5. ✅ Verify OLED displays readings
6. ✅ Press button SW5 — OLED should toggle ON/OFF
7. ✅ Open a BLE scanner app (e.g., nRF Connect) on phone
8. ✅ Find "MOAB-EnvMon" device and connect
9. ✅ Verify Environmental Sensing Service data updates
10. ✅ Optional: connect LiPo battery to Feather JST for portable operation
