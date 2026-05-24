# Project Journal: Arduino Environmental Monitor

### 2023-10-24: Project Conception & Requirements Definition
- **Goal**: Design a compact, reliable indoor environmental monitor.
- **Decisions**:
  - Chose the ATmega328P-AU microcontroller to keep the design compatible with the Arduino Uno/Nano ecosystem.
  - Selected the BME280 sensor over the DHT22 due to its superior accuracy and inclusion of barometric pressure readings.
  - Decided on a 0.96" SSD1306 OLED display for local visualization.
  - Opted for a 3.3V system architecture for the sensor and display, using an AMS1117-3.3V LDO regulator.

### 2023-10-25: Schematic Design & Component Selection
- **Activities**:
  - Drafted the schematic in KiCad.
  - Added decoupling capacitors (0.1uF) close to the ATmega328P VCC pins.
  - Configured the 16MHz crystal oscillator circuit with 22pF load capacitors.
  - Added 4.7kΩ pull-up resistors to the I2C lines (SDA/SCL).
  - Sourced all components from LCSC to ensure easy assembly and part availability.

### 2023-10-26: PCB Layout & Routing
- **Activities**:
  - Placed the BME280 sensor away from heat-generating components (like the AMS1117 regulator) to prevent thermal drift in temperature readings.
  - Routed the I2C lines with minimal vias to ensure signal integrity.
  - Created a solid ground plane on both the top and bottom layers.
  - Generated Gerber files and verified them using an online Gerber viewer.

### 2023-10-27: Firmware Development & Breadboard Validation
- **Activities**:
  - Set up a breadboard prototype using an Arduino Nano, a BME280 breakout board, and an OLED display.
  - Wrote the initial firmware using the Adafruit BME280 and SSD1306 libraries.
  - Resolved an I2C address conflict: the BME280 module was defaulting to `0x76` instead of `0x77`. Updated the code accordingly.
  - Verified that temperature, humidity, and pressure readings update correctly every 2 seconds.

### 2023-10-28: Final Review & Release
- **Activities**:
  - Finalized the Bill of Materials (BOM) and cross-checked LCSC part numbers.
  - Created the project documentation (`README.md`, `.gitignore`, and `JOURNAL.md`).
  - Prepared the project repository for release.