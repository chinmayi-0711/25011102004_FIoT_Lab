# CAT9 – STM32 UART Command Controlled LED

| Field | Details |
|-------|---------|
| EXP | CAT9 |
| DATE | 28/09/2026 |
| NAME OF EXP | STM32 UART Command Controlled LED |
| HARDWARE SETUP / PLATFORM | STM32F103C8T6 Blue Pill (ARM Cortex-M3, 72 MHz), ST-Link V2, USB-to-TTL serial adapter, STM32CubeMX + Keil MDK-ARM (HAL), PuTTY / Tera Term, on-board LED |
| TASK NO | Task 9A – Basic UART Communication, Task 9B – String-Based Application Logic |

## Overview

Configure a UART with STM32CubeMX and HAL, establish communication between the STM32 and a PC terminal, and build on it to control the on-board LED with text commands (`LED ON`, `LED OFF`, `LED TOGGLE`). The tasks progress from sending and echoing single characters to parsing complete command strings.

## HAL APIs Used

| HAL API | Purpose |
|---------|---------|
| `HAL_UART_Init()` | Initialise UART peripheral |
| `HAL_UART_Transmit()` | Transmit characters/string |
| `HAL_UART_Receive()` | Receive characters/string |
| `HAL_UART_Transmit_IT()` / `HAL_UART_Receive_IT()` | Interrupt-driven transfer (later) |
| `HAL_UART_Transmit_DMA()` / `HAL_UART_Receive_DMA()` | DMA transfer (advanced) |

## Tasks

| Task | Folder | Description |
|------|--------|-------------|
| Task 9A – Basic UART Communication | [TASK1/](TASK1/) | Transmit a character repeatedly; receive and echo every character |
| Task 9B – String-Based Application Logic | [TASK2/](TASK2/) | `LED ON` / `LED OFF` / `LED TOGGLE` commands, plus CR/LF investigation |

Each task folder contains its own `README.md` (objective, circuit diagram, core logic and inference), `circuit.jpeg` and the project files.
