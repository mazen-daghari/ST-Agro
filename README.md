# ST-Agro
This project is an STM32-based wireless data acquisition system with one Master/Gateway Node and multiple Sender Nodes. Sender Nodes collect and transmit data to the Gateway, which connects to a computer via USB. After each successful transmission, Sender Nodes enter 10-minute deep sleep to reduce power consumption.
# STM32 Master & Sender Node System

## Overview

This project implements a wireless sensor system based on **STM32 nodes**, consisting of a **Master (Gateway) Node** and multiple **Sender Nodes**.

The Master Node is connected to a computer through USB and acts as the gateway for receiving data from the Sender Nodes. Each Sender Node periodically collects and transmits data, then enters **deep sleep mode for 10 minutes** after a successful transmission to reduce power consumption.

---

## System Architecture

```text
                ┌─────────────────────┐
                │      Computer       │
                │   USB / COM Port    │
                └──────────┬──────────┘
                           │
                           │ USB
                           ▼
                ┌─────────────────────┐
                │   Master / Gateway  │
                │        Node         │
                └──────────┬──────────┘
                           │
              Wireless Communication
              ┌────────────┼────────────┐
              ▼            ▼            ▼
        ┌──────────┐ ┌──────────┐ ┌──────────┐
        │ Sender 1 │ │ Sender 2 │ │ Sender N │
        │   Node   │ │   Node   │ │   Node   │
        └──────────┘ └──────────┘ └──────────┘
```

---

# 1. Programming / Flashing the Nodes

Before testing the system, each node must be programmed using the **STM32-Link programmer**.

## 1.1 Flash the Master (Gateway) Node

1. Connect the **STM32-Link programmer** to the **Master Node PCB**.
2. Press and hold the **RESET** button for approximately **3 seconds**.
3. Select the **Master/Gateway Node** option in the programming software.
4. Click **Flash Device**.
5. Wait until the flashing process is completed successfully.
6. Disconnect the programmer after programming is complete.

## 1.2 Flash the Sender Node

1. Connect the **STM32-Link programmer** to the **Sender Node PCB**.
2. Select the **Sender Node** option.
3. Press and hold the **RESET** button for approximately **3 seconds**.
4. Click **Flash Device**.
5. Wait for the flashing process to complete successfully.
6. Disconnect the programmer.

## 1.3 Program Multiple Sender Nodes

If multiple Sender Nodes are used, repeat the same procedure for **each Sender Node**.

```text
Connect Programmer
       ↓
Select Node Type
       ↓
Press RESET for 3 sec
       ↓
Click "Flash Device"
       ↓
Wait for completion
       ↓
Repeat for next Sender Node
```

---

# 2. System Testing

After all nodes have been programmed, the system can be tested.

## 2.1 Connect the Master Node

1. Connect the **Master (Gateway) Node** to the computer using USB.
2. Open the system/software interface.
3. Refresh the **COM Port** list.
4. Find the COM Port corresponding to the Master Node.
5. Select the correct COM Port.
6. Click **Connect**.
7. The software should display a message confirming that the board has been **successfully connected**.

Example:

```text
COM Port: COMx
Status: Connected Successfully
```

---

# 3. Start the Sender Node

Once the Master Node is connected:

1. Power on the **Sender Node**.
2. The Sender Node will automatically start operating.
3. It will collect and transmit data to the **Master/Gateway Node**.
4. The received data can then be monitored through the computer interface.

No additional action is required to start data transmission after powering the Sender Node.

---

# 4. Deep Sleep Mode

To reduce power consumption, the Sender Node uses a **10-minute deep sleep cycle**.

After a successful data transmission:

```text
Data Acquisition
       ↓
Data Transmission
       ↓
Transmission Successful
       ↓
10-Minute Deep Sleep
       ↓
Wake Up
       ↓
Data Acquisition
       ↓
Data Transmission
       ↓
       ...
```

### Important

The **10-minute deep sleep period starts after a successful data transmission**.

During deep sleep, the Sender Node significantly reduces its power consumption. After 10 minutes, it automatically wakes up and resumes the data transmission cycle.

---

# 5. Complete Setup Procedure

For a complete system setup, follow these steps in order:

### Step 1 — Flash the Master Node

* Connect the STM32-Link programmer.
* Connect it to the Master Node PCB.
* Press **RESET** for approximately 3 seconds.
* Select the **Master/Gateway Node** option.
* Click **Flash Device**.
* Wait for the programming process to finish.

### Step 2 — Flash the Sender Nodes

For every Sender Node:

* Connect the STM32-Link programmer.
* Select **Sender Node**.
* Press **RESET** for approximately 3 seconds.
* Click **Flash Device**.
* Wait for the flashing process to finish.
* Repeat for all remaining Sender Nodes.

### Step 3 — Connect the Gateway

* Connect the Master Node to the computer via USB.
* Refresh the COM Port list.
* Select the correct COM Port.
* Click **Connect**.
* Verify that the software reports a successful connection.

### Step 4 — Power the Sender Nodes

* Power on the Sender Node(s).
* The nodes will automatically begin transmitting data.
* Monitor the received data through the Master/Gateway interface.

### Step 5 — Verify the Sleep Cycle

After a successful transmission:

* The Sender Node enters **deep sleep for 10 minutes**.
* After 10 minutes, it wakes up.
* The node resumes data collection and transmission automatically.

---

# 6. Troubleshooting

### Master Node is not detected

Check the following:

* USB connection.
* STM32-Link connection.
* Correct COM Port.
* Master Node power supply.
* Try pressing and holding **RESET** for 3 seconds.

### Sender Node does not transmit data

Check:

* Sender Node power supply.
* Correct firmware has been flashed.
* Sender Node has been programmed using the **Sender Node** option.
* Wireless communication between the Sender and Master Nodes.
* Wait for the node to complete its current sleep cycle.

### No data appears on the computer

Verify that:

1. The Master Node is connected to the correct COM Port.
2. The software reports **Connected Successfully**.
3. The Sender Node is powered on.
4. The Sender Node is not currently in the 10-minute deep sleep period.
5. The Sender Node has successfully completed its firmware flashing process.

---

# 7. Quick Start

For experienced users, the complete procedure is:

```text
1. Flash Master/Gateway Node
        ↓
2. Flash every Sender Node
        ↓
3. Connect Master Node via USB
        ↓
4. Refresh COM Ports
        ↓
5. Select Master COM Port
        ↓
6. Click Connect
        ↓
7. Power Sender Node(s)
        ↓
8. Receive Data
        ↓
9. Sender sleeps for 10 minutes
        ↓
10. Sender wakes up and transmits again
```

---

## Requirements

* STM32 Master/Gateway PCB
* STM32 Sender Node PCB(s)
* STM32-Link programmer
* USB cable
* Computer
* Appropriate power supply for the Sender Node(s)
* Programming and monitoring software

---

## Notes

* Make sure the correct node type is selected before flashing.
* The **Master/Gateway firmware** must be flashed to the Master Node.
* The **Sender firmware** must be flashed separately to each Sender Node.
* The 10-minute deep sleep cycle is automatically activated after each successful data transmission.
* When using multiple Sender Nodes, repeat the flashing procedure for every node.
