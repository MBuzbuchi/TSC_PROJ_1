# TSC_PROJ_1

E_Book proj

# _** Bloc Scheme**_
![image](https://github.com/user-attachments/assets/2968349c-9f69-43eb-b035-581131a2e7cd)

# _**Description of Hardware Functionality**_

Below is an extended list of the hardware components identified in the schematic, along with each component’s role and functionality, 
plus some operational details. These explanations help clarify how the entire system is built and why these design choices were made.

## 1. USB-C Connector & ESD Protection

### Key Components
- **USBLC6-2SC6Y** – TVS (Transient Voltage Suppressor) diode specialized for USB data line protection.  
  - **Approx. Price**: \$0.10–\$0.30 (single unit)  
- **PFMF.050.1** – resettable fuse (PTC) protecting the power line from excessive current.  
  - **Approx. Price**: \$0.10–\$0.20  
- **R2-USB1 / R2-USB** (5 kΩ) – matching/termination resistors on the USB D+ and D– data lines.  
  - **Approx. Price**: \$0.01–\$0.02 each  

### Function
- The USB-C connector supplies both power (5 V) to the system and USB data connectivity.  
- The ESD protection diode safeguards the USB data lines from electrostatic discharges up to several kV, maintaining the integrity of the data signals and internal converters.  
- The PTC fuse automatically resets once the current drops, preventing damage when short circuits or overcurrent situations occur on the USB port.

### Operational Data
- 5 V power supply from the USB, typically up to 500 mA (USB 2.0), or more depending on the USB-C source.  
- Data transfer rates up to 480 Mbps (USB 2.0 High Speed).

---

## 2. Voltage Regulators (LDO Voltage Regulator)

### Key Components
- **XC6220A331MR-G** – an LDO (Low Dropout) regulator delivering a stable 3.3 V output.  
  - **Approx. Price**: \$0.20–\$0.50  
- **DMG2305UX-7** – P-channel MOSFET used for power switching (possibly for reverse voltage protection or for reducing quiescent current when switching between USB and battery power).  
  - **Approx. Price**: \$0.10–\$0.30  

### Function
- Provides a constant 3.3 V rail for powering the ESP32-C6 and other logic circuits/sensors.  
- Can also serve as an additional protection stage, keeping the supply stable and filtering noise that may come from the USB source.

### Operational Data
- Input voltage range: ~3.5 V to 5.5 V (according to the LDO specs).  
- Output current up to ~300 mA.  
- Built-in overcurrent and thermal shutdown protection, ensuring circuit safety.

---

## 3. Li-Po Charging Controller (MCP73831/73832)

### Key Component
- **MCP73831** – a simple charge-management IC, configurable by an external resistor (PROG) to set the charge current.  
  - **Approx. Price**: \$0.50–\$0.80  

### Function
- Safely charges a single-cell Li-Po battery, regulating up to 4.2 V and controlling a maximum charge current.  
- Integrates temperature monitoring (via external NTC or dedicated resistor), charge status indicator (LED), and automatically switches to a float maintenance mode once the battery is fully charged.

### Operational Data
- Typically powered by a 5 V USB input.  
- Charge current (set by the PROG resistor) – e.g., 100 mA, 500 mA, etc., depending on the battery requirements.  
- Over-voltage, over-temperature, and short-circuit protection.

---

## 4. Single-Cell Li-Po Battery

### Function
- Stores electrical energy to power the device, enabling operation without a continuous USB supply.

### Operational Data
- Nominal voltage of ~3.7 V, fully charged at 4.2 V.  
- Capacity depends on the specific battery (500 mAh, 1000 mAh, etc.), which affects run time.  
- **Approx. Price**: Ranges from \$3 to \$8 or more, depending on capacity/brand.

---

## 5. ESP32-C6 Microcontroller (ESP32-C6-WROOM-1-N8)

### Function
- Core processing unit, running firmware and handling wireless communication (Wi-Fi 6, Bluetooth LE), as well as peripheral interfaces (SPI, I2C, UART, GPIO).  
- Integrates a 160 MHz RISC-V CPU, onboard Flash, SRAM, and hardware security features.

### Operational Data
- Powered at 3.3 V.  
- Low-power modes for efficient battery operation, ideal for portable IoT applications.  
- Interfaces with the E-Paper display (SPI), SD card (SPI), BME688 sensor (I2C), DS3231 RTC (I2C), and MAX17048 fuel gauge (I2C).  
- **Approx. Price**: \$2.50–\$5.00 (depending on memory size and supplier)

---

## 6. SD Card Interface

### Key Components
- MicroSD socket with card detection pins (CD, DETECTION_1, DETECTION_2).  
  - **Approx. Price**: \$0.40–\$1.00

### Function
- Provides a removable storage option for logging data, configuration files, E-Paper images, etc.  
- Uses SPI lines (SCK, MISO, MOSI, SS_SD) for communication with the ESP32-C6.

### Operational Data
- Operates at 3.3 V.  
- Supports SD/SDHC cards up to multiple GB, depending on firmware support.
- SD card itself (not included in the socket cost) can range from \$3 to \$20+ depending on capacity and speed class.

---

## 7. E-Paper Drive Circuit

### Key Components
- **MBR0530** Schottky diodes (e.g., D3, D4, D5) for protection and rectification in the voltage-boosting section.  
  - **Approx. Price**: \$0.05–\$0.10 each  
- **Inductor L1** (68 µH) for boost converter operation.  
  - **Approx. Price**: \$0.10–\$0.30 (depends on current rating)  
- **SI1308EDL-T1-GE3** MOSFET for switching in the high-voltage generator.  
  - **Approx. Price**: \$0.15–\$0.30  

### Function
- Creates the specialized positive and negative voltages (EPD_VGH, EPD_VGL) required by the E-Paper display.  
- Converts 3.3 V into higher and/or negative voltages needed to update the E-Ink display.

### Operational Data
- Typical output ranges: +15…22 V and -15…22 V (depending on the specific panel).  
- MBR0530 diodes have a low forward drop and can handle relatively high currents for this type of circuit.

---

## 8. Display Type Selector

### Key Components
- Jumper **SJ1**, resistor **R2** (2.2 kΩ).  
  - **Approx. Price**: Jumper \$0.05–\$0.10, resistor \$0.01–\$0.02

### Function
- Allows configuration of control pins (e.g. **RES**, **BUSY**) or power lines to support various E-Paper panel types.  
- Moving the jumper can route or enable certain lines depending on the display’s requirements.

---

## 9. E-Paper Display Header

### Component
- FFC (Flat Flex Cable) connector, **FH34SRJ-24S-0.5SH** (24-pin) or similar.  
  - **Approx. Price**: \$0.40–\$0.80

### Function
- A mechanical and electrical interface between the main board and the E-Paper module.  
- Includes SPI signals (SCK, MOSI, MISO—if bi-directional), control signals (CS, DC), reset line (RES), busy line (BUSY), and power lines (3.3 V, GND, EPD_VGH, EPD_VGL).

---

## 10. Battery Fuel Gauge (MAX17048)

### Key Component
- **MAX17048G+T10** – Fuel gauge with an I²C interface.  
  - **Approx. Price**: \$1.20–\$2.00

### Function
- Measures the Li-Po battery voltage and estimates its state of charge (SOC) in real time, compensating for temperature and discharge characteristics.  
- Allows the microcontroller to display battery level and react to critical low-battery states.

### Operational Data
- I²C interface at 3.3 V.  
- ±1–2% SOC accuracy.  
- Configurable alert output for low battery.

---

## 11. External Flash Memory (W25Q512JVEIQ, 64MB)

### Function
- Additional non-volatile storage (up to 64Mb or 512Mbit, i.e. 8 MB, depending on suffix) for images, fonts, configuration files, and Over-the-Air (OTA) firmware updates.

### Operational Data
- SPI interface with speeds up to 133 MHz.  
- 3.3 V supply voltage.  
- Up to 100,000 write/erase cycles and 20-year data retention.  
- **Approx. Price**: \$1.50–\$3.00 (depending on density and supplier)

---

## 12. SPI ESD Protection Lines

### Key Components
- **PGB1010603MR** (Polymeric ESD Suppressor) and 10 kΩ series resistors.  
  - **Approx. Price**: \$0.05–\$0.15 each for the suppressors, \$0.01 for each resistor

### Function
- Protects SPI data lines from electrostatic discharges, which can occur when plugging in peripherals or when the lines are touched without proper handling.  
- The 10 kΩ resistors help limit transient current before it reaches the TVS diode, providing additional filtering.

### Operational Data
- ESD protection up to ±8 kV (contact discharge).

---

## 13. Qwiic / Stemma QT Connector (I2C)

### Component
- 4-pin connector (GND, 3.3 V, SDA, SCL).  
  - **Approx. Price**: \$0.30–\$0.60

### Function
- Provides a standardized, plug-and-play interface for quickly adding I2C-based sensors or other modules.

### Operational Data
- Compatible with SparkFun Qwiic and Adafruit Stemma QT systems.  
- 3.3 V supply voltage.

---

## 14. RTC Module (DS3231SN)

### Key Component
- **DS3231SN** – A high-accuracy real-time clock with an integrated temperature-compensated oscillator (TCXO).  
  - **Approx. Price**: \$1.50–\$3.00

### Function
- Maintains precise timekeeping with minimal drift (±2 ppm, equivalent to ±1 minute/year in some conditions).  
- Communicates with the microcontroller via I²C for automatic time syncing.  
- Can keep time even when main power is off, using a backup battery on VBAT.

### Operational Data
- 3.3 V supply voltage.  
- Operating temperature range from -40 °C to +85 °C (depending on the specific version).

---

## 15. Test Pads

### Component
- Test points (TP1…TP17) placed around the PCB.  
  - **Approx. Price**: Usually negligible (just copper pads), sometimes \$0.01–\$0.02 each if you count plating cost.

### Function
- Provide direct access to key signals (SPI, I2C, GPIO) for debugging, measurements, programming, or firmware updates.  
- Highly useful for prototyping, quality control, and mass production validation.

---

# _**Bill of Materials**_
Data sheets and links to websites were included only at the first mention of components that appear multiple times in the project. Components such as test points (TP), do not have tehnical description or present a data sheet because these were designed personally


| Name of component  | Device                                    | Check Prices                                                                 | DataSheet                                                                  |
|--------------------|-------------------------------------------|-----------------------------------------------------------------------------|----------------------------------------------------------------------------|
| BOOT_BUTTON        | BUTTON_CUSYOMV1                           | [Check Price](https://industry.panasonic.com/global/en/products/control/switch/light-touch/number/evqpuj02k)  | [DataSheet](https://industry.panasonic.com/global/en/products/control/switch/light-touch/number/evqpuj02k) |
| C1                 | ESP32_WROVER_EAGLE-LTSPICE_CC0402         | [Check Price](https://industry.panasonic.com/global/en/products/control/switch/light-touch/number/evqpuj02k)  | [DataSheet](https://industry.panasonic.com/global/en/products/control/switch/light-touch/number/evqpuj02k) |
| C1_BAT             | ESP32_WROVER_EAGLE-LTSPICE_CC0402         | #N/A                                                                         | #N/A                                                                      |
| C1_BAT1            | EAGLE-LTSPICE_CC0402                      | #N/A                                                                         | #N/A                                                                      |
| C1_BAT2            | EAGLE-LTSPICE_CC0402                      | #N/A                                                                         | #N/A                                                                      |
| C2                 | ESP32_WROVER_EAGLE-LTSPICE_CC0402         | #N/A                                                                         | #N/A                                                                      |
| C2_BAT\            | ESP32_WROVER_EAGLE-LTSPICE_CC0402         | #N/A                                                                         | #N/A                                                                      |
| C3                 | RCL_CPOL-EUCT3528                         | #N/A                                                                         | #N/A                                                                      |
| C4                 | ESP32_WROVER_EAGLE-LTSPICE_CC0402         | #N/A                                                                         | #N/A                                                                      |
| C4_USB             | ESP32_WROVER_EAGLE-LTSPICE_CC0402         | #N/A                                                                         | #N/A                                                                      |
| C5                 | ESP32_WROVER_EAGLE-LTSPICE_CC0402         | #N/A                                                                         | #N/A                                                                      |
| C5_USB             | ESP32_WROVER_EAGLE-LTSPICE_CC0402         | #N/A                                                                         | #N/A                                                                      |
| C6                 | ESP32_WROVER_EAGLE-LTSPICE_CC0402         | #N/A                                                                         | #N/A                                                                      |
| C7                 | ESP32_WROVER_EAGLE-LTSPICE_CC0402         | #N/A                                                                         | #N/A                                                                      |
| C8                 | ESP32_WROVER_EAGLE-LTSPICE_CC0402         | #N/A                                                                         | #N/A                                                                      |
| C9                 | EAGLE-LTSPICE_CC0402                      | #N/A                                                                         | #N/A                                                                      |
| C10                | ESP32_WROVER_EAGLE-LTSPICE_CC0402         | #N/A                                                                         | #N/A                                                                      |
| C10_SUPERCAP       | CPH3225A                                  | [Check Price](https://www.snapeda.com/parts/CPH3225A/Seiko+Instruments/view-part/?ref=eda) | [DataSheet](https://www.snapeda.com/parts/CPH3225A/Seiko+Instruments/view-part/?ref=eda) |
| CHANGE_BUTTON      | BUTTON_CUSYOMV1                           | [Check Price](https://industry.panasonic.com/global/en/products/control/switch-light-touch/number/evqpuj02k)  | [DataSheet](https://industry.panasonic.com/global/en/products/control/switch-light-touch/number/evqpuj02k) |
| CHG_LED            | ADAFRUIT_LEDCHIP-LED0603                  | [Check Price](https://www.snapeda.com/parts/KP-1608SURCK/Kingbright/view-part/?ref=search&t=LED%200603) | [DataSheet](https://www.snapeda.com/parts/KP-1608SURCK/Kingbright/view-part/?ref=search&t=LED%200603) |
| C_DELAY            | ESP32_WROVER_EAGLE-LTSPICE_CC0402         | #N/A                                                                         | #N/A                                                                      |
| D1                 | USBLC6-2SC6Y                              | [Check Price](https://www.snapeda.com/parts/USBLC6-2SC6Y/STMicroelectronics/view-part/?ref=eda) | [DataSheet](https://www.snapeda.com/parts/USBLC6-2SC6Y/STMicroelectronics/view-part/?ref=eda) |
| D2                 | ESP32_WROVER_AVX---SD0805S020S1R0_AVX_... | [Check Price](https://eu.mouser.com/ProductDetail/KYOCERA-AVX/SD0805S020S1R0?qs=jCA%252BPfw4LHbpkAoSnwrdjw%3D%3D) | [DataSheet](http://datasheets.avx.com/schottky.pdf)                       |
| D3                 | MBR0530                                   | [Check Price](https://eu.mouser.com/ProductDetail/KYOCERA-AVX/SD0805S020S1R0?qs=jCA%252BPfw4LHbpkAoSnwrdjw%3D%3D) | [DataSheet](https://eu.mouser.com/ProductDetail/KYOCERA-AVX/SD0805S020S1R0?qs=jCA%252BPfw4LHbpkAoSnwrdjw%3D%3D) |
| D4                 | MBR0530                                   | [Check Price](https://www.snapeda.com/parts/MBR0530/Onsemi/view-part/?ref=eda) | [DataSheet](https://www.snapeda.com/parts/MBR0530/Onsemi/view-part/?ref=eda) |
| D5                 | MBR0530                                   | [Check Price](https://www.snapeda.com/parts/MBR0530/Onsemi/view-part/?ref=eda) | [DataSheet](https://www.snapeda.com/parts/MBR0530/Onsemi/view-part/?ref=eda) |
| D6                 | PGB1010603MR                              | [Check Price](https://www.snapeda.com/parts/PGB1010603MR/Littelfuse/view-part/?ref=eda) | [DataSheet](https://www.snapeda.com/parts/PGB1010603MR/Littelfuse/view-part/?ref=eda) |
| D7                 | ESP32_WROVER_AVX---SD0805S020S1R0_AVX_... | [Check Price](https://eu.mouser.com/ProductDetail/KYOCERA-AVX/SD0805S020S1R0?qs=jCA%252BPfw4LHbpkAoSnwrdjw%3D%3D) | [DataSheet](http://datasheets.avx.com/schottky.pdf)                       |
| D8                 | PGB1010603MR                              | [Check Price](https://www.snapeda.com/parts/PGB1010603MR/Littelfuse/view-part/?ref=eda) | [DataSheet](https://www.snapeda.com/parts/PGB1010603MR/Littelfuse/view-part/?ref=eda) |
| D9                 | PGB1010603MR                              | [Check Price](https://www.snapeda.com/parts/PGB1010603MR/Littelfuse/view-part/?ref=eda) | [DataSheet](https://www.snapeda.com/parts/PGB1010603MR/Littelfuse/view-part/?ref=eda) |
| D10                | PGB1010603MR                              | [Check Price](https://www.snapeda.com/parts/PGB1010603MR/Littelfuse/view-part/?ref=eda) | [DataSheet](https://www.snapeda.com/parts/PGB1010603MR/Littelfuse/view-part/?ref=eda) |
| D11                | PGB1010603MR                              | [Check Price](https://www.snapeda.com/parts/PGB1010603MR/Littelfuse/view-part/?ref=eda) | [DataSheet](https://www.snapeda.com/parts/PGB1010603MR/Littelfuse/view-part/?ref=eda) |
| D12                | PGB1010603MR                              | [Check Price](https://www.snapeda.com/parts/PGB1010603MR/Littelfuse/view-part/?ref=eda) | [DataSheet](https://www.snapeda.com/parts/PGB1010603MR/Littelfuse/view-part/?ref=eda) |
| EPD_C1             | ESP32_WROVER_EAGLE-LTSPICE_CC0402         | #N/A                                                                         | #N/A                                                                      |
| EPD_C2             | ESP32_WROVER_EAGLE-LTSPICE_CC0402         | #N/A                                                                         | #N/A                                                                      |
| EPD_C5             | ESP32_WROVER_EAGLE-LTSPICE_CC0402         | #N/A                                                                         | #N/A                                                                      |
| EPD_C6             | ESP32_WROVER_EAGLE-LTSPICE_CC0402         | #N/A                                                                         | #N/A                                                                      |
| EPD_C7             | ESP32_WROVER_EAGLE-LTSPICE_CC0402         | #N/A                                                                         | #N/A                                                                      |
| EPD_C8             | ESP32_WROVER_EAGLE-LTSPICE_CC0402         | #N/A                                                                         | #N/A                                                                      |
| EPD_C9             | ESP32_WROVER_EAGLE-LTSPICE_CC0402         | #N/A                                                                         | #N/A                                                                      |
| EPD_C10            | ESP32_WROVER_EAGLE-LTSPICE_CC0402         | #N/A                                                                         | #N/A                                                                      |
| EPD_C11            | ESP32_WROVER_EAGLE-LTSPICE_CC0402         | #N/A                                                                         | #N/A                                                                      |
| EPD_C12            | ESP32_WROVER_EAGLE-LTSPICE_CC0402         | #N/A                                                                         | #N/A                                                                      |
| IC1                | BD5229G-TR                                | [Check Price](https://componentsearchengine.com/part-view/BD5229G-TR/ROHM%20Semiconductor) | [DataSheet](https://componentsearchengine.com/part-view/BD5229G-TR/ROHM%20Semiconductor) |
| IC4                | XC6220A331MR-G                            | [Check Price](https://componentsearchengine.com/part-view/XC6220A331MR-G/Torex) | [DataSheet](https://componentsearchengine.com/part-view/XC6220A331MR-G/Torex) |
| J1                 | FH34SRJ-24S-0.5SH_99_                     | [Check Price](https://componentsearchengine.com/part-view/XC6220A331MR-G/Torex) | [DataSheet](https://componentsearchengine.com/part-view/XC6220A331MR-G/Torex) |
| J2                 | SAMACSYS_PARTS_USB4110-GF-A               | [Check Price](https://componentsearchengine.com/part-view/USB4110-GF-A/GCT%20(GLOBAL%20CONNECTOR%20TECHNOLOGY) | [DataSheet](https://componentsearchengine.com/part-view/USB4110-GF-A/GCT%20(GLOBAL%20CONNECTOR%20TECHNOLOGY) |
| J3                 | QWIIC_CONNECTORJS-1MM                     | #N/A                                                                         | #N/A                                                                      |
| J4                 | 112A-TAAR-R03_ATTEND                      | [Check Price](https://store.comet.srl.ro/Catalogue/Product/43497/)           | [DataSheet](https://store.comet.srl.ro/Catalogue/Product/43497/)          |
| L1                 | 744043680IND_4828-WE-TPC_WRE              | [Check Price](https://eu.mouser.com/ProductDetail/Wurth-Elektronik/744043680?qs=PGXP4M47uW6VkZq%252BkzjrHA%3D%3D) | [DataSheet](https://eu.mouser.com/ProductDetail/Wurth-Elektronik/744043680?qs=PGXP4M47uW6VkZq%252BkzjrHA%3D%3D) |
| PFMF.050.1         | ESP32C6_VARISTORCN1812                    | [Check Price](https://www.mouser.co.uk/ProductDetail/EPCOS-TDK/B72520T0350K062?qs=dEfas%2FXlABIszF52uu7vrg%3D%3D) | [DataSheet](https://www.mouser.co.uk/ProductDetail/EPCOS-TDK/B72520T0350K062?qs=dEfas%2FXlABIszF52uu7vrg%3D%3D) |
| Q1                 | ESP32_WROVER_SPARKFUN-DISCRETESEMI_MOSFET_... | [Check Price](https://componentsearchengine.com/part-view/DMG2305UX-7/Diodes%20Incorporated) | [DataSheet](https://componentsearchengine.com/part-view/DMG2305UX-7/Diodes%20Incorporated) |
| Q2                 | ESP32_WROVER_SPARKFUN-DISCRETESEMI_MOSFET_... | #N/A                                                                         | #N/A                                                                      |
| Q3                 | D8                                        | [Check Price](https://componentsearchengine.com/part-view/PGB1010603MR/Littelfuse) | [DataSheet](https://componentsearchengine.com/part-view/PGB1010603MR/Littelfuse) |
| Q4                 | Q2_TS-B2_REACHPACK                        | #N/A                                                                         | #N/A                                                                      |

# _**Pin Allocation for ESP32-C6**_

| Pin ESP32-C6  | Componentă / Semnal                       | Explicație                                           |
|---------------|-------------------------------------------|------------------------------------------------------|
| GPIO1         | I²C SDA (BME688, MAX17048, DS3231, Qwiic) | Se folosește pentru a partaja magistrala I²C între toți senzorii și RTC. |
| GPIO2         | I²C SCL (BME688, MAX17048, DS3231, Qwiic) | Linia de clock pentru comunicarea I²C.              |
| GPIO5         | SPI MISO (E-Paper)                       | Pinul pentru citirea datelor sau a statusului de la ecranul e-paper. |
| GPIO6         | SPI MOSI (E-Paper)                       | Trimiterea datelor către ecranul e-paper.            |
| GPIO7         | SPI CLK (E-Paper)                        | Semnal de clock pentru sincronizarea cu e-paper.    |
| GPIO8         | SPI CS (E-Paper)                         | Chip Select pentru a activa comunicarea cu e-paper.  |
| GPIO9         | E-Paper DC (Data/Command)                | Semnal pentru a diferenția între date și comenzi pentru ecranul e-paper. |
| GPIO10        | E-Paper RST                              | Pinul de reset hardware pentru e-paper.              |
| GPIO11        | E-Paper BUSY                             | Intrare din display care indică dacă ecranul este ocupat sau nu. |
| GPIO12        | Button #1                                | Intrare digitală pentru primul buton (ex. pagina următoare). |
| GPIO13        | Button #2                                | Intrare digitală pentru al doilea buton (ex. pagina anterioară). |
| GPIO14        | Button #3                                | Intrare digitală pentru al treilea buton (ex. Meniu/OK). |
| GPIO15        | MAX17048 ALERT (opțional)                | Semnal de alertă de la MAX17048 pentru nivel scăzut al bateriei. |
| GPIO16        | USB D+ (intern la USB PHY)               | Pin dedicat pentru semnalul USB 2.0 (D+).            |
| GPIO17        | USB D- (intern la USB PHY)               | Pin dedicat pentru semnalul USB 2.0 (D-).            |
| GPIO18        | LED de status (opțional)                 | Indicator pentru diverse stări (ex. Wi-Fi, încărcare, etc.). |
| GPIO19        | SD Card CS (opțional)                    | Chip Select pentru cardul SD (dacă se folosește SPI). |
| GPIO20        | SD Card MISO (opțional)                  | Intrare date de la cardul SD.                        |
| GPIO21        | SD Card MOSI (opțional)                  | Ieșire date către cardul SD.                         |
| GPIO4         | SD Card CLK (opțional)                   | Semnal de clock pentru cardul SD (SPI).              |





