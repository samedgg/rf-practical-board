## Project Overview

This project introduces a highly integrated, four-layer printed circuit board meticulously designed to process and consolidate multiple radio frequency signals, including Wi-Fi, Bluetooth, and GPS, into a single practical platform. At the core of the system is an STM32F4 series microcontroller handling the main control logic, seamlessly paired with an ESP32-C3-MINI-1 module for robust 2.4 GHz wireless communication and a NEO-M8T module for precise satellite navigation. The architecture leverages a strict four-layer stackup comprising dedicated signal layers on the top and bottom, a solid internal ground plane, and a split 3.3V power plane to ensure maximum signal integrity and low impedance power delivery across the system. Furthermore, the board features a comprehensive array of modern interfaces, including a USB Type-C receptacle for reliable power delivery, a MicroSD card socket for extensive data logging, and various communication buses such as UART, I2C, and SPI to accommodate diverse external peripherals and analog sensors.

## 3D Views and Schematics

The physical layout and component placement were optimized for both mechanical fit and electrical isolation, ensuring that high-frequency modules do not interfere with sensitive analog domains. The hierarchical schematic design visually breaks down the complex system into manageable logical blocks, separating the main MCU, power distribution, and RF sections.

**3D Board Renders:**

<img width="820" height="775" alt="3d" src="https://github.com/user-attachments/assets/b236de06-acb7-401f-8eef-e02edf565179" />
<img width="820" height="775" alt="3d back" src="https://github.com/user-attachments/assets/8ec52863-0846-489c-b07b-89ec4e6c08eb" />


**Schematics:**
Top Sheet Architecture: 
<img width="3509" height="2481" alt="top sheet" src="https://github.com/user-attachments/assets/4ee41d21-6df8-4130-8843-32e5985af018" />

MCU, SD Card and Flash:
<img width="3509" height="2481" alt="MCU,SD,FLASH" src="https://github.com/user-attachments/assets/4e92a940-38a7-4064-920c-26669c32b8d0" />

RF and Sensors:
<img width="3509" height="2481" alt="RF_Sensors" src="https://github.com/user-attachments/assets/e826f47e-90e0-45c3-983b-89a9d604a478" />

Power Management:
<img width="3509" height="2481" alt="Power" src="https://github.com/user-attachments/assets/cd83192e-93eb-40bc-a6e4-1960c31bd079" />

Connectors:
<img width="3509" height="2481" alt="Connectors" src="https://github.com/user-attachments/assets/5e0d58cc-3a84-41c9-81c0-81998962a790" />


## Layers

The layer stackup was engineered to provide optimal return paths and thermal dissipation. The top and bottom layers handle the intricate routing of mixed-signal traces, while the internal layers are entirely dedicated to solid copper pours for power and ground, virtually eliminating ground loops and significantly reducing radiated emissions.

<img width="868" height="771" alt="layers" src="https://github.com/user-attachments/assets/36c76ab5-a58a-4e2e-b309-c26acedb29cb" />

<img width="868" height="771" alt="l1" src="https://github.com/user-attachments/assets/acc4f89a-3231-4228-a9d7-6297722ed901" />

<img width="868" height="771" alt="l2" src="https://github.com/user-attachments/assets/f136f261-a421-46d5-82a3-b2bc9dbb8770" />

<img width="868" height="771" alt="l3" src="https://github.com/user-attachments/assets/5d2318ff-1d6b-427f-a5c2-6f5fb024359c" />

<img width="868" height="771" alt="l4" src="https://github.com/user-attachments/assets/90973234-0cf1-47de-9bd5-f6f6d87c5026" />


## Bill of Materials

| Designator | Component | Description |
| :--- | :--- | :--- |
| **U1** | STM32F4 Series | 32-bit ARM Cortex-M4 Microcontroller |
| **U2** | ESP32-C3-MINI-1 | Wi-Fi & Bluetooth LE Module |
| **U3** | NEO-M8T-0 | GPS / GNSS Module |
| **U4** | AP7361C-33L-13 | 3.3V LDO Voltage Regulator |
| **U5** | MT3608 | 5V Step-Up (Boost) Converter |
| **J4** | MCP73831T-2ACI/OT | Li-Po Charge Management Controller |
| **J1** | U.FL-R-SMT-1(10) | RF Antenna Connector |
| **J5** | TYPE-C-31-M-12 | USB Type-C Receptacle |
| **SD1** | 503182-1852 | MicroSD Card Socket |
| **U2 (Flash)** | W25Q128JVSIQ | 128M-bit Serial Flash Memory |
| **C17, C18, R14** | Pi-Matching Network | DNI (Do Not Install) 0402 components for future VNA tuning |

## Highlights

### 1. Critical Trace Length & Wavelength Calculation
In high-frequency designs, treating a trace as a simple wire rather than a distributed transmission line is only permissible if its physical length is significantly shorter than the critical length threshold ($L_c$), typically defined as $\lambda/12$ or $\lambda/10$. For the GPS L1 signal operating at $1575.42\text{ MHz}$, propagating through a microstrip with an effective dielectric constant ($\varepsilon_{eff}$) of approximately $3.4$, the critical length is calculated using the formula $L_c = \frac{c}{f} \cdot \frac{1}{\sqrt{\varepsilon_{eff}}} \cdot \frac{1}{12}$. This yields a critical length limit of exactly $8.6\text{ mm}$. The actual routed RF trace on this board was strictly measured in Altium to be $16\text{ mil}$ ($0.406\text{ mm}$) between the matching network pads, deliberately kept orders of magnitude below the $8.6\text{ mm}$ threshold to prevent phase shifts, reflections, and the need for complex transmission line modeling.
<img width="1781" height="331" alt="rf measured track" src="https://github.com/user-attachments/assets/037c58ff-cbf6-4cb5-967e-fc6ba301afd9" />

### 2. 50-Ohm Impedance Matching & CPWG Avoidance
The RF antenna path was designed as a meticulously calculated 50-ohm microstrip with a 4.575 mil trace width. To guarantee absolute impedance stability and eliminate the risk of Coplanar Waveguide (CPWG) parasitic capacitance, a strict 16-mil clearance was enforced between the sensitive RF trace and the surrounding Top Layer ground polygon. An in-line Pi-matching network (C-R-C) utilizing 0402 footprints was strategically integrated just before the U.FL connector and left unpopulated (DNI) to provide a flexible provision for future Vector Network Analyzer (VNA) tuning.
<img width="1218" height="764" alt="key_point1" src="https://github.com/user-attachments/assets/b428ef86-5117-4893-877f-b5703535ab71" />

### 3. Via Shielding (Faraday Cage)
The entire RF path is fortified by a densely packed via shielding fence. These vias are spaced at intervals much smaller than $\lambda/10$ and directly stitch the top ground pour to the solid L2 internal ground plane, creating a highly effective localized Faraday cage around the microstrip. This structure actively suppresses electromagnetic interference (EMI) radiated by adjacent digital switching, prevents unwanted substrate resonance, and ensures a pristine, low-impedance return path for the high-frequency analog signals.

### 4. Mixed-Signal Isolation & Solid Return Paths
To maintain absolute signal integrity across the system, the layout strictly enforces spatial and planar isolation. The sensitive GPS RF circuitry is physically isolated in the top-right quadrant of the board, completely distanced from the aggressive switching nodes of the DC-DC boost converter and the high-current ESP32 Wi-Fi module. Furthermore, Layer 2 is dedicated entirely as an unbroken, solid ground plane. This unbroken reference plane guarantees that all high-speed digital return currents flow directly underneath their respective traces, minimizing loop inductance, crosstalk, and common-mode radiation without polluting the analog ground regions.
