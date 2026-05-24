# Arduino Environmental Monitor (BME280 + OLED)

An open-source, compact, and highly accurate environmental monitoring system built around the ATmega328P microcontroller. It measures temperature, relative humidity, and barometric pressure, displaying the real-time data on a 0.96" I2C OLED display.

## Why

- **High Accuracy**: Uses the Bosch BME280 sensor, which outperforms standard DHT11/DHT22 sensors in precision, response time, and long-term stability.
- **Compact & Integrated**: Designed as a standalone custom PCB rather than a bulky breadboard or shield assembly, making it suitable for practical home deployment.
- **Low Power**: Utilizes a highly efficient LDO regulator (AMS1117-3.3V) and optimized I2C pull-ups to minimize power consumption for potential battery-powered operations.
- **Educational Value**: Serves as an excellent reference design for custom ATmega328P hardware integration, I2C bus routing, and sensor interfacing.

## How

### Hardware Architecture
- **Microcontroller**: ATmega328P-AU running at 16MHz (external crystal oscillator).
- **Power Supply**: 5V input (via USB or header) regulated down to 3.3V using the AMS1117-3.3V LDO to power the BME280 and OLED display.
- **Communication**: Both the BME280 and the SSD1306 OLED display share the I2C bus (SDA/SCL) with 4.7kΩ pull-up resistors to 3.3V.

### Software Setup
1. **Prerequisites**: Install the [Arduino IDE](https://www.arduino.cc/en/software) or PlatformIO.
2. **Libraries**: Install the following libraries via the Arduino Library Manager:
   - `Adafruit BME280 Library`
   - `Adafruit SSD1306`
   - `Adafruit GFX Library`
3. **Programming**: Connect an FTDI basic programmer (or any USB-to-UART adapter) to the board's serial programming header (TX, RX, DTR, VCC, GND). Select **Arduino Uno** or **Arduino Nano (ATmega328P)** in the IDE and upload the firmware.

### Firmware Example
cpp
#include <Wire.h>
#include <Adafruit_Sensor.h>
#include <Adafruit_BME280.h>
#include <Adafruit_SSD1306.h>

#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, -1);
Adafruit_BME280 bme;

void setup() {
  Wire.begin();
  if(!bme.begin(0x76)) { 
    // Check your I2C address (0x76 or 0x77)
    while(1);
  }
  if(!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
    while(1);
  }
  display.clearDisplay();
  display.setTextColor(SSD1306_WHITE);
}

void loop() {
  float temp = bme.readTemperature();
  float hum = bme.readHumidity();
  float pres = bme.readPressure() / 100.0F;

  display.clearDisplay();
  display.setCursor(0,0);
  display.setTextSize(1);
  display.println("ENV MONITOR");
  display.println("---------------------");
  display.print("Temp: "); display.print(temp); display.println(" C");
  display.print("Hum:  "); display.print(hum); display.println(" %");
  display.print("Pres: "); display.print(pres); display.println(" hPa");
  display.display();
  delay(2000);
}


## Bill of Materials (BOM) Summary

| Reference | Value | Footprint | Qty | LCSC Part # | Description |
|-----------|-------|-----------|-----|-------------|-------------|
| U1 | ATmega328P-AU | TQFP-32 | 1 | C14877 | 8-bit AVR Microcontroller |
| U2 | BME280 | LGA-8 | 1 | C94993 | Temp, Humidity & Pressure Sensor |
| U3 | AMS1117-3.3V | SOT-223-3 | 1 | C6186 | 3.3V LDO Voltage Regulator |
| Y1 | 16MHz | SMD-3225 | 1 | C13738 | Crystal Oscillator |
| C1, C2 | 22pF | C0805 | 2 | C1618 | Ceramic Capacitor |
| C3, C4 | 0.1uF | C0805 | 2 | C49308 | Decoupling Capacitor |
| R1, R2 | 10k | R0805 | 2 | C17414 | Pull-up Resistors (Reset/I2C) |
| R3, R4 | 4.7k | R0805 | 2 | C17626 | I2C Pull-up Resistors |
| J1 | OLED Header 1x4 | HDR-F-2.54 | 1 | C50981 | Female Header for OLED Module |