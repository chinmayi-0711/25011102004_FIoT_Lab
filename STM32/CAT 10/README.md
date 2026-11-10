# CAT10 – STM32–Wemos Wi-Fi Modem Interface Using AT Commands

| Field | Details |
|-------|---------|
| EXP | CAT10 |
| DATE | 05/10/2026 |
| NAME OF EXP | STM32–Wemos Wi-Fi Modem Interface Using AT Commands |
| HARDWARE SETUP / PLATFORM | STM32F103C8T6 Blue Pill (ARM Cortex-M3, 72 MHz), ST-Link V2, Wemos D1 Mini (ESP8266, AT firmware), USB-to-TTL serial adapter, STM32CubeMX + Keil MDK-ARM (HAL), PuTTY / Tera Term |
| TASK NO | Task 10A – STM32 ↔ Wemos UART Communication, Task 10B – Explore the ESP8266 AT Command Interface, Task 10C – Connect Wemos to Wi-Fi |

> The lab sheet labels these tasks 9A, 9B and 9C; they are numbered 10A–10C here to match CAT10.

## Overview

Interface an STM32 board with a Wemos D1 Mini (ESP8266) through UART, configure the Wi-Fi modem using AT commands, and connect it to a Wi-Fi network. The tasks first prove that the two boards can talk, then investigate the ESP8266 AT command set, and finally build firmware that puts the modem in station mode and joins an access point. This is the base for sending sensor data over Wi-Fi.

## UART Plan

| UART | Connection | Purpose |
|------|------------|---------|
| UART1 | STM32 ↔ Wemos | AT commands and responses |
| UART2 | STM32 ↔ PC | Debug / display |

## Tasks

| Task | Folder | Description |
|------|--------|-------------|
| Task 10A – STM32 ↔ Wemos UART Communication | [TASKA_STM32_Wemos_UART/](TASKA_STM32_Wemos_UART/) | Send `AT\r\n`, receive `OK` and display it on the PC terminal |
| Task 10B – Explore the ESP8266 AT Command Interface | [TASKB_AT_Command_Exploration/](TASKB_AT_Command_Exploration/) | Command investigation table for modem, Wi-Fi and TCP commands |
| Task 10C – Connect Wemos to Wi-Fi | [TASKC_Connect_to_WiFi/](TASKC_Connect_to_WiFi/) | Station mode, join AP, report mode / status / IP |

Each task folder contains its own `README.md` (objective, circuit diagram, core logic and inference), `circuit.png` and the project files.
