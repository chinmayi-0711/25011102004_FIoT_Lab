# CAT9 – Task 9B – String-Based Application Logic

| Field | Details |
|-------|---------|
| EXP | CAT9 |
| DATE | 28/09/2026 |
| NAME OF EXP | STM32 UART Command Controlled LED |
| HARDWARE SETUP / PLATFORM | STM32F103C8T6 Blue Pill (ARM Cortex-M3, 72 MHz), ST-Link V2, USB-to-TTL serial adapter, STM32CubeMX + Keil MDK-ARM (HAL), PuTTY / Tera Term, on-board LED |
| TASK NO | Task 9B – String-Based Application Logic |

## 1. Objective

Develop a UART-based command interface for the STM32. The system accepts commands from a terminal and controls the on-board LED:

| Command | Response |
|---------|----------|
| `LED ON` | LED turned ON |
| `LED OFF` | LED turned OFF |
| `LED TOGGLE` | LED toggled |

Also investigate `'\r'` (Carriage Return) and `'\n'` (Line Feed) using a small test program and explain why both are needed.

## 2. Circuit Diagram

![Circuit Diagram](circuit.png)

**Pin Connections:**

| Signal / Component | MCU Pin | Description |
|--------------------|---------|-------------|
| 3.3 V / GND | 3V3 / GND | Board supply from ST-Link / USB |
| HSE | 8 MHz crystal → PLL 72 MHz | System clock (configured in CubeMX) |
| USART2_TX | PA2 | STM32 → USB-TTL RX |
| USART2_RX | PA3 | USB-TTL TX → STM32 |
| LED | PC13 | Output (on-board LED, active-LOW) |
| GND | GND | Common ground with USB-TTL adapter |

UART settings: 115200 baud, 8N1, no flow control.

## 3. Core Logic / Pseudocode / Code Base

**Working Principle:**

Characters are received one at a time and stored in a buffer until the terminator (`'\r'` or `'\n'`) arrives. The buffer is then terminated with `'\0'` and compared with the known commands using `strcmp()`. A matching command drives PC13 and a confirmation string is sent back to the terminal.

**Pseudocode:**

1. Initialise HAL, system clock, GPIO and USART2; print a prompt (`> `).
2. Receive one character with `HAL_UART_Receive()`.
3. If it is not `'\r'` or `'\n'`, store it in the buffer (guard against overflow) and echo it.
4. On `'\r'` / `'\n'`, terminate the string and compare:
   - `"LED ON"` → `HAL_GPIO_WritePin(GPIOC, GPIO_PIN_13, GPIO_PIN_RESET)` (active-LOW: ON), reply `LED turned ON`.
   - `"LED OFF"` → `HAL_GPIO_WritePin(GPIOC, GPIO_PIN_13, GPIO_PIN_SET)`, reply `LED turned OFF`.
   - `"LED TOGGLE"` → `HAL_GPIO_TogglePin(GPIOC, GPIO_PIN_13)`, reply `LED toggled`.
   - Otherwise reply `Unknown command`.
5. Clear the buffer, print the prompt and repeat.

**CR / LF test program:** transmit each string with `HAL_UART_Transmit()` and record what PuTTY shows.

| Test | String sent | Behaviour in PuTTY |
|------|-------------|--------------------|
| 1 | `"HELLO\r\n"` | Text printed, cursor moves to the start of the next line |
| 2 | `"HELLO\r"` | Cursor returns to the start of the same line; next output overwrites `HELLO` |
| 3 | `"HELLO\n"` | Cursor moves down one line but stays in the same column (staircase effect) |
| 4 | `"HELLO\r\nWORLD\r\n"` | `HELLO` and `WORLD` appear on two clean separate lines |

**Code Base:**

Uses `HAL_UART_Receive()`, `HAL_UART_Transmit()`, `HAL_GPIO_WritePin()`, `HAL_GPIO_TogglePin()` and string handling from `<string.h>`.

## 4. Inference

**Observation:**

`LED ON`, `LED OFF` and `LED TOGGLE` each changed the LED as expected and the matching confirmation message appeared in the terminal. The CR/LF tests behaved as listed above.

**Conclusion:**

Collecting characters into a buffer and comparing the finished string gives a simple text command interface over UART. CR moves the cursor to the start of the line and LF moves it down one line; they are separate actions inherited from teletypes, so both (`\r\n`) are needed for a proper new line and are widely used as the end-of-line terminator in text protocols.
