# ST-Agro: Wireless Data Acquisition and Control System

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Platform](https://img.shields.io/badge/Platform-STM32-blue.svg)](https://www.st.com/)
[![Dashboard](https://img.shields.io/badge/GUI-St%20Agro%20Control%20Dashboard-green.svg)](https://youtu.be/QQxuJRQUxHk)
[![Promo](https://img.shields.io/badge/Video-Promotional%20Video-red.svg)](https://youtu.be/v6s37rHDwMU)

A low-power, STM32-based wireless data acquisition and control architecture designed for smart agriculture and industrial environmental monitoring. The system consists of a Master/Gateway Node connected to a PC via USB and multiple Sender (Sensor) Nodes deployed in the field. 

System capabilities include integrated SWD firmware deployment, live sensor telemetry (temperature, humidity, multi-gas detection), AI-driven threat assessment (fire/water hazard detection), GPS node mapping, real-time analytics charts, and remote actuator relay matrix control with emergency stop (E-STOP) functionality.

---

## Demonstration & Promotional Videos

* **Promotional Video:** [![ST-Agro Promotional Video](https://img.youtube.com/vi/v6s37rHDwMU/maxresdefault.jpg)](https://youtu.be/v6s37rHDwMU)
  
---

* **Dashboard Demonstration:** [![ST-Agro Dashboard Demo](https://img.youtube.com/vi/QQxuJRQUxHk/maxresdefault.jpg)](https://youtu.be/QQxuJRQUxHk)

*Click the thumbnails above to view the videos on YouTube.*

---

## System Architecture

![System Architecture](https://raw.githubusercontent.com/mazen-daghari/ST-Agro/644b52f263133617d76fb6110cbb068184cfff48/Architecture-svg.svg)
---

## Software & Hardware Visual Overview

The system relies on a unified desktop application built in Python using `customtkinter` for modern UI components, `matplotlib` for real-time analytics, `sqlite3` for local persistent storage, and `tkintermapview` for node mapping. The application acts as a dual-purpose software hub:

1. **Direct SWD Hardware Flashing:** Communicates directly with `STM32_Programmer_CLI.exe` to flash compiled binaries (`.elf`) to Master Node (Slot A) and Sender Nodes (Slot B) at base address `0x08000000` over SWD/JTAG interfaces.
2. **LoRa Gateway Supervision & Actuator Control:** Connects to the Master Node over Serial UART to parse incoming sensor telemetry, render live per-node metrics, display active GPS node positions, log background telemetry to SQLite (`node_telemetry.db`), and send remote 4-channel relay control commands or Emergency Stop (`E-STOP`) overrides.
### Application Software Interface

![Dashboard View](https://github.com/mazen-daghari/ST-Agro/blob/a745ba1f822ba48972e0608266926620c045ff7f/ST-AGRO%20DASGBOARD.png)
*Figure 1: ST-Agro Control Dashboard showcasing live telemetry metrics, hazard risk calculations, real-time node statuses, relay controls, and global emergency stop features.*

![Flasher View](https://github.com/mazen-daghari/ST-Agro/blob/a745ba1f822ba48972e0608266926620c045ff7f/ST-AGRO%20FLASHER.png)
*Figure 2: SWD Flasher Interface used to flash Master (Slot A) and Sender (Slot B) firmware directly from the desktop application.*

![Node Map View](https://github.com/mazen-daghari/ST-Agro/blob/a745ba1f822ba48972e0608266926620c045ff7f/ST-AGRO%20NODE%20MAP.png)
*Figure 3: Integrated OpenStreetMap view displaying active field node locations based on live transmitted GPS spatial data.*

![Analytics Charts View](https://github.com/mazen-daghari/ST-Agro/blob/a745ba1f822ba48972e0608266926620c045ff7f/ST-AGRO%20NODE%20CHARTS.png)
*Figure 4: Historical and real-time database aggregated analytics visualizing sensor trends and telemetry metrics per node.*

---

### Hardware PCB Layouts

![Master Node PCB](https://github.com/mazen-daghari/ST-Agro/blob/774fa99ae3891e5d04ee102ccba33a86722e32f9/ST-AGRO%20Master%20GATEWAY%20node.png)
*Figure 5: Master/Gateway Node PCB featuring the STM32 microcontroller, USB communication interface, and SWD programming header.*

![Sender Node PCB](https://github.com/mazen-daghari/ST-Agro/blob/774fa99ae3891e5d04ee102ccba33a86722e32f9/ST-AGRO%20Sender%20Node.png)

*Figure 6: Sender (Sensor) Node PCB integrating low-power STM32 architecture, sensor array interfaces, wireless transmitter, and power management circuits.*

---

## Core System Features

* **Integrated SWD Flasher Interface:** Deploy `Master Node (Slot A)` and `Sender Node (Slot B)` firmware binaries directly from the control desktop GUI without requiring external programming tools.
* **Low-Power Deep Sleep Cycle:** Sender nodes perform data collection, transmit packets to the Gateway, and immediately enter a 10-minute ultra-low-power deep sleep state to optimize battery consumption.
* **Multi-Sensor Telemetry Acquisition:**
  * Environmental Data: Temperature and Relative Humidity
  * Gas Array Monitoring: MQ2, MQ7, and MQ135 sensor payload values
* **Threat Intelligence Processing:** Real-time threat calculation providing automated risk scores for Fire Hazard and Water Quality alerts.
* **Actuator Control Matrix:** Remote execution and control of a 4-channel relay array (`R1: Irrigation`, `R2: Fire Pump`, `R3: Ventilation`, `R4: Alarm Buzzer`).
* **Emergency Stop (E-STOP):** Single-click software override that initiates an immediate hardware reset and emergency shutdown state across active nodes.
* **Node Geolocation Tracking:** Integrated OpenStreetMap spatial tracking displaying live GPS locations of active field nodes.
* **Database Aggregated Analytics:** Comprehensive historical and per-node real-time graphical metrics.

---

## Getting Started

### Hardware Requirements

1. Master/Gateway Node PCB (STM32-based controller)
2. Sender Node PCB(s) (STM32-based sensor nodes)
3. ST-LINK V2 / SWD Debugger
4. USB Cable (A-to-Micro or Type-C based on hardware design)
5. Sensor payload array (DHT/SHT series, MQ gas sensors, GPS module)
6. 4-Channel Relay Control Module

---

## Setup and Flashing Instructions

### Step 1: Flash the Master (Gateway) Node

1. Connect the ST-LINK programmer via the SWD header to the Master Node PCB.
2. Launch the **St Agro Programmer & Control Dashboard**.
3. Navigate to the **Flasher** tab on the left panel.
4. Under **Target Firmware**, select **Master Node (Slot A)**.
5. Set the debug interface to **SWD**.
6. Press and hold the hardware **RESET** button on the Master PCB for approximately 3 seconds, then click **Flash Device**.
7. Monitor the **System Event Log** until the process completes with the message `=== FLASH SUCCESSFUL ===`.

### Step 2: Flash the Sender Node(s)

1. Connect the ST-LINK programmer to the target Sender Node PCB.
2. In the **Flasher** tab, select **Sender Node (Slot B)**.
3. Hold the hardware **RESET** button on the Sender PCB for approximately 3 seconds, then click **Flash Device**.
4. Disconnect the debugging interface upon completion.
5. Repeat this sequence for each additional Sender Node within the network topology.

```text
Connect Programmer ──> Select Firmware Slot ──> Hold RESET (3s) ──> Click "Flash Device" ──> Verify Log Output
```

---

## System Operation and Communications

### Gateway Connection

1. Connect the programmed Master Node to the host computer using a USB cable.
2. In the desktop application under **Serial Connection**, click the refresh icon adjacent to the COM port menu.
3. Select the appropriate Master Node port (e.g., `COM3`) and set the baud rate to **38400**.
4. Click **Connect**. The status indicator in the upper right corner will confirm connection status (`Connected - COM3`).

### Node Deployment

1. Apply power to the Sender Node(s).
2. The Sender Node automatically collects sensor metrics and transmits telemetry packets to the Gateway Node.
3. Following each confirmed transmission, the node enters a **10-minute deep sleep cycle** prior to initiating the subsequent measurement cycle.

---

## Application Interface Overview

| Interface Section | Primary Function |
| :--- | :--- |
| **Dashboard** | Monitors active node state, hazard alerts (`FIRE_HAZARD`), live telemetry logs, relay matrix switching, and global **E-STOP** triggers. |
| **Flasher** | In-app firmware deployment interface for Master and Sender binaries via SWD. |
| **Node Map** | Geographical layout visualizing GPS coordinate data transmitted by deployed nodes. |
| **Charts & Global** | Graphical breakdown of historical database records and active per-node metric channels. |

---

## Troubleshooting Guide

* **COM Port Not Detected:** Verify FTDI/UART device driver installation and physical cable connection. Click the refresh control in the Serial Connection settings.
* **Firmware Flashing Failure:** Inspect SWD pin connections (`SWCLK`, `SWDIO`, `GND`, `3V3`). Ensure the physical **RESET** button is held for 3 seconds prior to clicking **Flash Device**.
* **Missing Telemetry Data:** Confirm the Sender node is powered and not currently operating within its 10-minute deep sleep phase. Press the physical RESET button on the Sender node to initiate an immediate transmission.

---
## Core System Features

* **Integrated SWD Flasher Interface:** Direct desktop firmware deployment via `STM32CubeProgrammer` CLI supporting SWD/JTAG modes for Master (`Slot A`) and Sender (`Slot B`) binaries targeting flash address `0x08000000`.
* **Low-Power Deep Sleep Cycle:** Sender nodes collect data, transmit packets to the Gateway, and immediately enter an ultra-low-power 10-minute deep sleep state to conserve battery.
* **Multi-Sensor Telemetry Acquisition:** 
  * Environmental Data: Temperature and Relative Humidity
  * Gas Array & Air Quality Monitoring: MQ2, MQ7, and MQ135 sensor payload values
* **AI-Driven Threat Intelligence:** Automated risk calculation for Fire Hazard (`AI Fire`) and Water Quality (`AI Water`) detection.
* **Actuator Control Matrix:** Remotely toggle a 4-channel relay matrix (`R1: Irrigation`, `R2: Fire Pump`, `R3: Ventilation`, `R4: Alarm Beacon`) on individual field nodes over LoRa.
* **Emergency Stop (E-STOP) Override:** Single-click software override per node that locks controls, turns off active relays, and broadcasts an emergency shutdown command (`NODE:<ID>:EMERGENCY_STOP`).
* **Node Geolocation Tracking:** Embedded OpenStreetMap module (`tkintermapview`) displaying real-time field positions based on live transmitted GPS coordinates.
* **SQLite Telemetry Logging & Analytics:** Automatic local database persistence (`node_telemetry.db`) with real-time per-node streaming charts and aggregated global network averages.
* **Hardware Interfacing Guide:** Built-in pinout reference tables for SWD (PA13/PA14), 20-pin JTAG, and ST-LINK/V2 dongles.
---
## Getting Started

### Hardware Requirements

1. Master/Gateway Node PCB (STM32-based controller)
2. Sender Node PCB(s) (STM32-based sensor nodes)
3. ST-LINK V2 / SWD Debugger
4. USB Cable (A-to-Micro or Type-C based on hardware design)
5. Sensor payload array (DHT/SHT series, MQ gas sensors, GPS module)
6. 4-Channel Relay Control Module

### Software Requirements
* Install and run St Agro Programmer V2.32
---
## Copyright and License

This software and hardware implementation is released under the **MIT License**.

```text
MIT License

Copyright (c) 2026 Mazen Daghari

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

### Citation Notice

For academic, scientific, or technical research projects utilizing this design, please cite the developer as follows:

* **Author:** Electronic & Communication System Engineer **Mazen Daghari**
* **Project:** ST-Agro Wireless Acquisition System & Control Dashboard
* **Demonstration Reference:** https://youtu.be/QQxuJRQUxHk
 ### Paper & Citation
If you find this work useful in your research, please cite our paper:

> M. Daghari, "ST-Agro: Low-Power Wireless Data Acquisition System," *IEEE...*
> **Preprint / Paper:** [Link]
