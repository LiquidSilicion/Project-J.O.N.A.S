# Project J.O.N.A.S (Just Off-grid Navigational and Alert System) 🧭✨

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Platform: ESP32](https://img.shields.io/badge/Platform-ESP32--IDF-green)](https://www.espressif.com/en/products/socs/esp32)
[![Technology: BLE](https://img.shields.io/badge/Technology-Bluetooth%20Low%20Energy-blue)](https://www.bluetooth.com/)

**A standalone, privacy-focused wearable device that helps small groups locate each other in crowded, GPS-denied environments without relying on smartphones or cellular networks.**

---

## Table of Contents

- [The Problem](#the-problem)
- [The Solution](#the-solution)
- [Features](#features)
- [Hardware](#hardware)
- [Software & Architecture](#software--architecture)
- [Installation & Getting Started](#installation--getting-started)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Contributing](#contributing)
- [License](#license)
- [Acknowledgements](#acknowledgements)

---

## The Problem

In large-scale gatherings like concerts, festivals, or tourist attractions, getting separated from your group is a common and stressful experience. Conventional solutions fail when they are needed most:
- **GPS** is inaccurate indoors and in dense urban areas.
- **Wi-Fi/Cellular** networks are often congested or unavailable.
- **Bluetooth Trackers** only show proximity, not direction, and rely on a smartphone.

**Project JONAS** was born to solve this specific challenge.

## The Solution

Project JONAS is an open-source, wearable device that creates a private, offline mesh network for your group. By fusing Bluetooth Low Energy (BLE) signal strength with an onboard compass, it provides intuitive, real-time directional guidance to any group member.

**How it works in a nutshell:**
1.  **Pair** your devices with a simple button press.
2.  **Select** a group member on your device.
3.  **Rotate** slowly; the ring of colored LEDs will point you in their direction.
4.  **Walk** towards the light to reunite!

It's designed to be used by anyone, from children to the elderly, with minimal instruction.

## Features

- **🔒 Offline & Private:** No internet, GPS, or central servers required. Your location data never leaves the group.
- **🧭 Intuitive Directional Guidance:** A circular LED ring points the way like a visual compass. Each member has a unique color.
- **📡 Extended Range with Mesh Networking:** Devices relay signals through the group, effectively multiplying the communication range.
- **🆘 Emergency SOS Alert:** A dedicated button triggers immediate visual and haptic alerts on all group devices.
- **🔋 Low Power Consumption:** Built with BLE and power-efficient components for all-day use.
- **👥 Scalable Groups:** Supports small to medium-sized groups (e.g., families, friends) effectively.
- **💡 Open Source:** Fully open hardware and software for transparency, learning, and community development.

## Hardware

The device is built around commonly available, low-cost components.

| Component | Purpose | Recommended Part |
| :--- | :--- | :--- |
| **Microcontroller** | System brain, BLE communication | ESP32 S3 |
| **IMU Sensor** | Tilt-compensated compass heading | MPU-6050 + QMC5883L (or BNO055) |
| **Display** | Proximity, status, and menu info | 0.96" SSD1306 I2C OLED Display |
| **LED Indicator** | Primary directional interface | 12x or 24x WS2812B Neopixel Ring |
| **Haptic Feedback** | Alerts and confirmations | 3V Vibration Motor (Disc Type) |
| **User Input** | Control, menu, SOS function | Tactile Push Buttons |
| **Power** | Battery & Management | 500mAh LiPo + TP4056 Charger Module |

*Detailed wiring diagrams and BOM available in the `hardware` directory.*

## Software & Architecture

### Core Protocol
1.  **Advertising & Scanning:** Each device periodically broadcasts a BLE advertisement packet containing a randomized Group ID, its own Device ID, and status flags.
2.  **Bearing Estimation:** When searching for a member, the device samples the Received Signal Strength Indicator (RSSI) of the target's packets while the user rotates. This data is fused with compass readings from the IMU using a weighted averaging algorithm to calculate the precise bearing.
3.  **Mesh Relaying:** Devices automatically rebroadcast packets from other group members with a hop-count, extending the network range.

### Key Libraries
- **ESP-IDF/Arduino Core for ESP32**: BLE stack and system control
- **Adafruit_NeoPixel**: LED ring control
- **Adafruit_SSD1306**: OLED display driver
- **Sensor Fusion Libraries**: IMU data processing

## Installation & Getting Started

### Prerequisites
- **Hardware:** Components listed in [Hardware](#hardware) section
- **Software:**
    - [Arduino IDE](https://www.arduino.cc/en/software) with **ESP32 Board Support**
    - OR [VS Code](https://code.visualstudio.com/) with [PlatformIO](https://platformio.org/) (Recommended)

### Flashing the Firmware
1.  **Clone the repository:**
    ```bash
    git clone https://github.com/your-username/project-jonas.git
    cd project-jonas/firmware
    ```
2.  **Open the Project:**
    - In **Arduino IDE**: Open `project_jonas.ino`
    - In **PlatformIO**: Open the `firmware` directory
3.  **Install Libraries** via Library Manager
4.  **Connect and Upload** to your ESP32

## Usage

1.  **Power On** all devices
2.  **Form a Group:** Hold "Pair" button on devices until LED confirmation
3.  **Navigate:** Select a member - LEDs point in their direction
4.  **SOS:** Press/hold SOS button for emergency alert

## Project Structure

**project-jonas/**  
├── **firmware/**                 *# Source code*  
│   ├── **src/**  
│   │   ├── main.cpp         *# Main application*  
│   │   ├── ble_handler.cpp  *# BLE communication*  
│   │   ├── imu_handler.cpp  *# Compass & sensors*  
│   │   ├── ui_handler.cpp   *# LEDs, Display, Buttons*  
│   │   └── mesh_protocol.cpp*# Packet relaying*  
│   └── **lib/**  
├── **hardware/**                *# Design files*  
│   ├── **schematics/**          *# Circuit diagrams*  
│   ├── **pcb/**                 *# PCB layouts*  
├── **docs/**                    *# Documentation*  
│   ├── protocol_spec.md     *# Communication protocol*  
│   └── algorithm_details.md *# Bearing estimation*  
└── README.md               *# This file*  
