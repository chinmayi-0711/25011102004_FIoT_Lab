# CAT10 – Task 10B – Explore the ESP8266 AT Command Interface

| Field | Details |
|-------|---------|
| EXP | CAT10 |
| DATE | 05/10/2026 |
| NAME OF EXP | STM32–Wemos Wi-Fi Modem Interface Using AT Commands |
| HARDWARE SETUP / PLATFORM | STM32F103C8T6 Blue Pill (ARM Cortex-M3, 72 MHz), ST-Link V2, Wemos D1 Mini (ESP8266, AT firmware), USB-to-TTL serial adapter, STM32CubeMX + Keil MDK-ARM (HAL), PuTTY / Tera Term |
| TASK NO | Task 10B – Explore the ESP8266 AT Command Interface |

## 1. Objective

Using the ESP8266 AT-command documentation, work out which command satisfies each requirement in the command investigation table (modem check, firmware version, station mode, scan, join AP, network info, TCP connection and sending data), and test them from the STM32.

## 2. Circuit Diagram

![Circuit Diagram](circuit.png)

**Pin Connections:**

| Signal / Component | Connection | Description |
|--------------------|------------|-------------|
| Wemos TX | STM32 UART1 RX | Data from Wemos to STM32 |
| Wemos RX | STM32 UART1 TX | Data from STM32 to Wemos |
| STM32 UART2 | USB-to-TTL adapter → PC | Debug / display |
| GND | GND | Common ground reference |
| 5V / 3.3V | 5V / 3.3V | Appropriate power source |

| UART | Connection | Purpose |
|------|------------|---------|
| UART1 | STM32 ↔ Wemos | AT commands and responses |
| UART2 | STM32 ↔ PC | Debug / display |

## 3. Core Logic / Pseudocode / Code Base

**Working Principle:**

Every AT command is a text string ending in `\r\n`. The modem answers with data followed by `OK` or `ERROR`. The STM32 sends one command at a time, reads the reply until `OK`/`ERROR` (or a timeout) and prints it on the PC terminal.

**Command Investigation Table:**

| Requirement | Command |
|-------------|---------|
| Check modem response | `AT` |
| Check firmware/version | `AT+GMR` |
| Set Wi-Fi station mode | `AT+CWMODE=1` |
| Scan available networks | `AT+CWLAP` |
| Connect to an AP | `AT+CWJAP="SSID","password"` |
| Obtain network information | `AT+CIFSR` |
| Establish TCP connection | `AT+CIPSTART="TCP","host",port` |
| Send application data | `AT+CIPSEND=<length>`, then the data |

**Pseudocode:**

1. Initialise HAL, system clock and both UARTs.
2. For each command in the table: transmit `command + "\r\n"` to the Wemos.
3. Collect the reply until `OK`, `ERROR` or a timeout.
4. Forward the reply to the PC terminal and note the result.

**Code Base:**

Uses `HAL_UART_Transmit()` / `HAL_UART_Receive()` with a timeout and a helper that sends a command and returns the response text.

## 4. Inference

**Observation:**

`AT` returned `OK`, `AT+GMR` printed the firmware/SDK version, `AT+CWMODE=1` returned `OK`, `AT+CWLAP` listed nearby access points, and `AT+CIFSR` reported the station IP/MAC after the Wi-Fi connection was made.

**Conclusion:**

The ESP8266 acts as a Wi-Fi modem controlled entirely by text AT commands. Reading the command reference lets a host such as the STM32 configure the mode, join a network, open a TCP connection and send data without implementing the Wi-Fi stack itself.
