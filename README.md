# Livestock Protection Device (LPD)

## Overview
The Livestock Protection Device is a hardware-based early warning system designed to mitigate human-wildlife conflict in rural ecosystems. By utilizing a dual-sensor array (motion and vibration), the system acts as a localized deterrent, triggering auditory and visual alarms to protect livestock from approaching threats while conserving power for prolonged field deployment.

## Features
* **Dual-Threat Detection:** Uses a PIR motion sensor and a vibration sensor to detect both spatial movement and physical disturbances (e.g., fence rattling).
* **Active Deterrence:** Triggers a high-decibel siren and a bright LED strobe to scare off potential wildlife threats.
* **Low Power Optimization:** Utilizes AVR sleep modes (`SLEEP_MODE_PWR_DOWN`) to minimize battery consumption when the perimeter is secure, making it ideal for off-grid rural deployment.
* **Failsafe Error Handling:** Continuously monitors sensor health. If sensors fail to provide valid readings consecutively, the system halts and logs the error to prevent false positives or silent failures.

## Hardware Requirements
* 1x Arduino-compatible Microcontroller (AVR-based, e.g., Arduino Uno/Nano)
* 1x PIR Motion Sensor
* 1x Vibration Sensor (e.g., SW-420)
* 1x Alarm/Siren Module
* 1x LED Strobe Light Module
* Jumper wires and power supply (Battery pack recommended for field use)

## Pin Configuration
| Component | Pin Number | Type |
| :--- | :--- | :--- |
| PIR Motion Sensor | `2` | Input |
| Vibration Sensor | `3` | Input |
| Siren | `4` | Output |
| LED Strobe | `5` | Output |

## Installation & Usage
1. **Wire the Components:** Connect the sensors, siren, and strobe to the microcontroller according to the pin configuration table above.
2. **Flash the Code:** 
   * Open `LivestockProtection.ino` in the Arduino IDE.
   * Select your specific board and COM port.
   * Click **Upload**.
3. **Monitor the System:** You can open the Serial Monitor (set to `9600` baud) to view real-time system initialization, threat detection logs, and power-state changes.

## How It Works
Upon initialization, the device enters a continuous monitoring loop. If either the PIR or the vibration sensor detects activity, the system activates the siren and strobe for 5 seconds to deter the threat. If the perimeter is clear, the system puts the microcontroller into a deep sleep state to conserve energy, waking up only when a new threat is detected.

## Future Improvements
* Integration of `attachInterrupt()` for seamless waking from deep sleep.
* Addition of GSM/LoRa modules to send automated SMS alerts to farmers when the system is triggered.
