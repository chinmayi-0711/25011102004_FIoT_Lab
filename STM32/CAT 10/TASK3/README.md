# CAT10 – Task 10C – Connect Wemos to Wi-Fi

| Field | Details |
|-------|---------|
| EXP | CAT10 |
| DATE | 05/10/2026 |
| NAME OF EXP | STM32–Wemos Wi-Fi Modem Interface Using AT Commands |
| HARDWARE SETUP / PLATFORM | STM32F103C8T6 Blue Pill (ARM Cortex-M3, 72 MHz), ST-Link V2, Wemos D1 Mini (ESP8266, AT firmware), USB-to-TTL serial adapter, STM32CubeMX + Keil MDK-ARM (HAL), PuTTY / Tera Term |
| TASK NO | Task 10C – Connect Wemos to Wi-Fi |

## 1. Objective

Develop STM32 firmware that configures the Wemos ESP8266 as a Wi-Fi station and connects it to the university/local Wi-Fi network using AT commands, then reports the mode, status and IP address on the PC terminal.

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

## 3. Core Logic / Pseudocode / Code Base

**Working Principle:**

The STM32 drives a fixed sequence of AT commands and waits for the reply to each before moving on:

```
STM32
  |
  |-- AT                  -> Response?
  |-- Configure station mode
  |-- Join AP
  |-- Wait for response
  '-- Query network status
```

**Pseudocode:**

1. Send `AT` and wait for `OK` (modem alive).
2. Send `AT+CWMODE=1` and wait for `OK` (station mode).
3. Send `AT+CWJAP="<SSID>","<password>"` and wait for `WIFI CONNECTED`, `WIFI GOT IP` and `OK` (allow a long timeout; joining takes several seconds).
4. Send `AT+CIFSR` to read the IP address.
5. Print the verification summary to the PC terminal.

**Expected terminal output:**

```
STM32-WiFi MODEM TEST
>> AT
<< OK
>> AT+CWMODE=1
<< OK
>> AT+CWJAP="MyWiFi","password"
<< WIFI CONNECTED
<< WIFI GOT IP
<< OK

WIFI MODE   : STATION
WIFI STATUS : CONNECTED
IP ADDRESS  : xxx.xxx.xxx.xxx
```

**Code Base:**

Uses `HAL_UART_Transmit()` / `HAL_UART_Receive()` with per-command timeouts and a helper that sends a command and checks the reply for the expected keyword. Replace the SSID and password with placeholders before pushing to GitHub; do not commit real credentials.

## 4. Inference

**Observation:**

The Wemos replied `OK` to `AT` and `AT+CWMODE=1`, reported `WIFI CONNECTED` and `WIFI GOT IP` after `AT+CWJAP`, and the STM32 printed `WIFI MODE : STATION`, `WIFI STATUS : CONNECTED` and the assigned IP address.

**Conclusion:**

An ESP8266 can be configured and connected to a Wi-Fi network purely through AT commands issued by the STM32. Each step must wait for its response (with a suitable timeout) before the next one is sent, and the connection can be verified by querying the IP address.
