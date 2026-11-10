# CAT12 – Task 12B – On-board Switch Control with State Remembrance

| Field | Details |
|-------|---------|
| EXP | CAT12 |
| DATE | 14/10/2026 |
| NAME OF EXP | STM32F401RE Cortex-M4: GPIO and User Input Programming |
| HARDWARE SETUP / PLATFORM | STM32 NUCLEO-F401RE (STM32F401RET6, ARM Cortex-M4, up to 84 MHz), on-board ST-LINK/V2-1, STM32CubeMX + Keil MDK-ARM (HAL) |
| TASK NO | Task 12B – On-board Switch Control with State Remembrance |

## 1. Objective

Modify the application so that the on-board push button controls the LED. The LED toggles its state whenever a valid button press is detected and remembers its previous state after the button is released.

## 2. Circuit Diagram

![Circuit Diagram](circuit.png)

**Pin Connections:**

| Signal / Component | MCU Pin | Description |
|--------------------|---------|-------------|
| Power / programming | USB (ST-LINK) | Board supply and debug link over the on-board ST-LINK |
| System clock | HSE (8 MHz from ST-LINK) or HSI → PLL | Configured in CubeMX |
| LED (LD2) | PA5 | Output (on-board user LED, active-HIGH) |
| User button (B1) | PC13 | Input (on-board, external pull-up; reads LOW when pressed) |

## 3. Core Logic / Pseudocode / Code Base

**Working Principle:**

Instead of making the LED follow the button level, the firmware reacts to the press *event* (released → pressed) and toggles the LED. The LED output keeps its new level after the button is released, so the state is remembered. A short debounce with `HAL_GetTick()` prevents contact bounce from causing extra toggles.

**Pseudocode:**

1. Initialise HAL, system clock and GPIO; set `prev = released`.
2. Read `curr` from PC13 (`GPIO_PIN_RESET` = pressed).
3. If `prev == released` and `curr == pressed`, record `t = HAL_GetTick()`.
4. If still pressed after ~50 ms, toggle the LED with `HAL_GPIO_TogglePin(GPIOA, GPIO_PIN_5)`.
5. Wait for release, update `prev = curr`, repeat.

**Code Base:**

Uses `HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_13)`, `HAL_GPIO_TogglePin()` and `HAL_GetTick()` for edge detection and debouncing.

## 4. Inference

**Observation:**

Each press toggled the LED once, and the LED stayed in its new state after the button was released.

**Conclusion:**

Detecting the press event with debouncing, rather than reading the level, lets the LED hold its state between presses and gives exactly one toggle per press.
