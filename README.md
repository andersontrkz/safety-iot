# Safety IoT - Smart Safe System

## Table of Contents
- [Overview](#overview)
- [System Architecture](#system-architecture)
- [Repository Structure](#repository-structure)
- [Features](#features)
  - [ESP32 Firmware](#esp32-firmware)
  - [Android App](#android-app)
- [Getting Started](#getting-started)
  - [Firmware (ESP32)](#firmware-esp32)
  - [App (Android)](#app-android)
- [Hardware Components](#hardware-components)
- [Communication Protocol](#communication-protocol)
- [Development Workflow](#development-workflow)
- [Security Considerations](#security)
- [Troubleshooting](#troubleshooting)
- [Future Improvements](#future-improvements)
- [License](#license)

---

## Overview

**Safety IoT** is a secure smart safe system integrating:

- **ESP32-based IoT device firmware** (PlatformIO/Wokwi)
- **Android mobile app** (Jetpack Compose)

The system allows **real-time monitoring**, secure local access with code entry, and simulation using Wokwi.

---

## System Architecture

```bash
+------------+              +-----------+
|            |              |           |
|   ESP32    |     MQTT     |  ANDROID  |
|  FIRMWARE  | <----------> |    APP    |
|            |              |           |
+------------+              +-----------+
```


**Components**:

- **ESP32 Firmware**: C++ / PlatformIO / Wokwi
- **Android App**: Kotlin / Jetpack Compose / Paho MQTT
- **MQTT Broker**: Public HiveMQ broker for messaging

---


## Repository Structure
````
safety/
│
├── mobile/
│ └── android/ ← Android app submodule
│
├── firmware/
│ └── esp32/ ← ESP32 firmware submodule
│
├── .gitmodules ← Git submodules configuration
└── README.md
````

> Uses **Git Submodules** for modular separation of Android app and ESP32 firmware.

-----


## Features

### ESP32 Firmware
- Keypad input handling for code entry
- Servo-controlled lock mechanism
- LED & buzzer hardware feedback
- LCD messages for user interaction
- Code setup and verification logic
- Supports mute/unmute and lock actions
- WiFi connectivity management
- MQTT messaging (`safer/status`)

### App Android
- Displays safe status (locked/unlocked)
- Shows last access events
- Built with Jetpack Compose
- Real-time updates via MQTT (`safer/status`)

---


## Getting Started

Clone repository including submodules:

```bash
git clone --recurse-submodules git@github.com:andersontrkz/safety.git
cd safety
```

### ESP32 Firmware

        1. Open firmware/esp32 in VS Code with PlatformIO OR run simulation in Wokwi.

        2. Or run simulation in Wokwi.

        3. Build & upload to ESP32.

        4. Configure WiFi config if needed.

        5. Configure MQTT config if needed.

        6. Monitor serial logs for system feedback.

### Android App

        7. Open mobile/android in Android Studio.

        8. Build and run on emulator or physical device.

        9. Configure MQTT config if needed.

        10. The app automatically connects to the MQTT broker and displays safe status.


## Hardware Components

```bash
+----------+         +---------------+
|  Keypad  | ------> |               |
+----------+         |               |
+----------+         |               |
|   LCD    | <------ |               |
+----------+         |               |
+----------+         |               |
|   Servo  | <------ |     ESP32     |
+----------+         |               |
+----------+         |               |
|   LEDs   | <------ |               |
+----------+         |               |
+----------+         |               |
|  Buzzer  | <------ |               |
+----------+         +---------------+

```

|     Component     |        Pin        |
| ----------------- | ----------------- |
| Keypad (R1..R4)   | 13 - 12 - 14 - 27 |
| Keypad (C1..C4)   | 26 - 25 - 33 - 32 |
| LCD (I2C)         | 22 - 21           |
| Servo Motor       | 18                |
| LED (RED)         | 4                 |
| LED (GREEN)       | 16                |
| Buzzer            | 17                |
| ESP32             | MCU               |


## Communication Protocol

- MQTT Topic: ``safer/status``
- Payload: ``LOCKED`` or ``UNLOCKED``

*App subscribes to this topic in real-time.*


## Development Workflow

### ESP32 Firmware changes:

#### ESP32 changes:
```bash
git add .
git commit -m "Feature XYZ"
git push origin feature/xyz
```

#### Update ESP32 submodule in main repo:
```bash
cd ../../
git add firmware/esp32

git commit -m "feat(esp32): update module"
git push
```


### Android app changes:

#### Android changes:
```bash
cd mobile/android

git commit -m "feat(android): update module"
git push
```

#### Update Android submodule in main repo:
```bash
cd ../../
git add mobile/android

git commit -m "feat(android): update module"
git push
```

## Security

- Safe code
    - Storage class in-memory
    - Fixed 4-digit code


## Troubleshooting

- WiFi issues
    - Check WiFiConfig.h 
    - Check serial monitor

- MQTT connection failed
    - Check verify broker 
    - Check network

- LCD not displaying
    - Check verify broker 
    - Check network

- LCD not displaying:
    - Check SDA pin
    - Check SCL pin

- Keypad unresponsive:
    - Check keypad setup


## Future Improvements

- Encrypted MQTT communication (TLS)

- App-based unlock with OTP authentication

- Multi-user access logging

- Low-power / battery monitoring

- Safe availability feedback

- ROM-based memory implementation


## License

[![MIT License](https://img.shields.io/badge/License-MIT-green.svg)](https://choosealicense.com/licenses/mit/)
