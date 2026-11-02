# ESP32 FPV Flight Controller

A modular PlatformIO project for a Betaflight-inspired ESP32 freestyle FPV flight controller.

## Features
- ESP32 dual-core task split:
  - Core 1: sensor sampling, gyro filtering, PID loop, motor mixing
  - Core 0: CRSF/ELRS, telemetry, GPS, web server, Wi-Fi AP
- Supports MPU6000 / MPU6500 / ICM-series via SPI, with I2C fallback
- 1 kHz to 2 kHz IMU and PID loop
- Betaflight-style rate profiles:
  - acro
  - horizon
  - angle
  - rates / super-rates / expo
- PID tuning with D-term on measurement
- Dynamic filtering:
  - PT1
  - biquad low-pass
  - notch
- Feed-forward (FF)
- TPA
- Anti-Gravity
- Airmode with mixer desaturation logic
- DShot300 / DShot600 motor output via ESP32 RMT
- Web configurator served over Wi-Fi AP
- GPS, barometer, compass module toggle support
- Multi-stage failsafe support

## Architecture
This codebase intentionally separates concerns:

- `core/`: FreeRTOS and dual-core scheduler infrastructure
- `flight/`: rates, PID, mixer, modes, failsafe
- `sensors/`: IMU + gyro/filter pipeline
- `esc/`: DShot output
- `rc/`: CRSF/ELRS receiver and command parsing
- `web/`: Wi-Fi AP + web config server
- `data/configurator/`: static web configurator UI

## Flashing
1. Install PlatformIO
2. `pio run`
3. `pio run -t upload`
4. Open serial monitor

## Accessing the web configurator
The board creates a Wi-Fi AP:
- SSID: `FPV-FC-XXXX`
- Password: `fpvfc123`

Then visit:
- `http://192.168.4.1/`

## Important note
This repository is a production-minded architecture scaffold. The exact tuning values, gyro calibration thresholds, and firmware defaults should be refined against your chosen ESCs, prop setup, and flight test data.

## Hardware assumptions
- IMU: MPU6000/6500 or ICM20689/42688
- ESC protocol: DShot300 / DShot600
- Receiver: CRSF/ELRS serial
- Optional: GPS, barometer, magnetometer