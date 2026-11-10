# CAT12 – STM32F401RE Cortex-M4: GPIO and User Input Programming

| Field | Details |
|-------|---------|
| EXP | CAT12 |
| DATE | 14/10/2026 |
| NAME OF EXP | STM32F401RE Cortex-M4: GPIO and User Input Programming |
| HARDWARE SETUP / PLATFORM | STM32 NUCLEO-F401RE (STM32F401RET6, ARM Cortex-M4, up to 84 MHz), on-board ST-LINK/V2-1, STM32CubeMX + Keil MDK-ARM (HAL), on-board LED (LD2) and user push-button (B1) |
| TASK NO | Task 12A – On-board LED Bring-Up, Task 12B – On-board Switch Control with State Remembrance, Task 12C – Short-Press and Long-Press Detection |

> The lab sheet is titled Continuous Lab Evaluation-11 (Week 11); it is recorded here as CAT12 and its Tasks 1–3 are numbered 12A–12C.

## Overview

Bring up the NUCLEO-F401RE board and program its on-board LED and user button using HAL GPIO. The tasks progress from a fixed-interval blink, to debounced button toggling with state memory, to distinguishing short and long presses.

## Prerequisites (Board Study)

| Item | Details |
|------|---------|
| Controller | STM32F401RET6, Cortex-M4 with FPU, up to 84 MHz |
| Oscillator | 8 MHz from ST-LINK (MCO) as HSE, or internal 16 MHz HSI; PLL used to reach 84 MHz |
| GPIO groups | Ports A, B, C, D, E, H |
| Communication peripherals | 3 × USART, 3 × I2C, 4 × SPI |
| On-board LED / button | LD2 on PA5, user button B1 on PC13 (active-LOW) |
| Reference | Nucleo-64 user manual UM1724 (pinout for STM32F401RE) |

Clock configuration in CubeMX: select the clock source (HSE bypass or HSI), set the PLL for the required SYSCLK (up to 84 MHz) and confirm the clock tree has no errors.

## Tasks

| Task | Folder | Description |
|------|--------|-------------|
| Task 12A – On-board LED Bring-Up | [TASKA_Onboard_LED_BringUp/](TASKA_Onboard_LED_BringUp/) | Blink the on-board LED at a fixed interval |
| Task 12B – Switch Control with State Remembrance | [TASKB_Switch_Control_State_Memory/](TASKB_Switch_Control_State_Memory/) | Each valid press toggles the LED; state is kept after release |
| Task 12C – Short-Press and Long-Press Detection | [TASKC_Short_vs_Long_Press/](TASKC_Short_vs_Long_Press/) | Different actions for short and long presses |

Each task folder contains its own `README.md` (objective, circuit diagram, core logic and inference), `circuit.png` and the project files.
